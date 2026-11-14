---
type: source
title: "PiSAs: Benchmarking Contextual Integrity in Multi-User Agentic Systems"
tags: [agents, benchmark, contextual-integrity, llm-memory, defenses]
authors:
  - Shubham Gupta
  - Nazanin Mohammadi Sepahvand
  - Abhinav Kumar
  - Cem Subakan
  - Spandana Gella
  - Pierre-André Noël
  - Perouz Taslakian
  - Eugene Bagdasarian
  - Valentina Zantedeschi
venue: arXiv
year: 2026
status: ingested
url: https://arxiv.org/abs/2607.05318
ingested: 2026-11-14
created: 2026-11-12
updated: 2026-11-14
---

# PiSAs

> [!info] Source
> Gupta et al. arXiv 2607.05318, cs.MA and cs.CR; v1 submitted 6 Jul 2026, ==ingested from v2== (PDF stamped 19 Oct 2026). No venue is shown. Code and data: `github.com/ServiceNow/pisas`, `huggingface.co/datasets/ServiceNow/PiSAs`. Read from a text extraction of the v2 PDF: main body and Appendices A–G in full. ==Not read:== Appendix H formulas, Appendices I–P (per-task results, prompt wording, specialized-model tables 13–14, memory tables 15–17, aggregation, worked example) and all figure contents (Figures 3, 4, 5, 7 are images). No Zotero annotations found; `zotero` citekey not set.

> [!caution] v1 and v2 differ substantially
> v1 had 85 scenarios in three IT tasks, three backbones (Claude Sonnet 4.6, gpt-oss-120b, Qwen3.6-27B) and no §5.4. Its numbers (for example single-agent appropriateness violations of 75–95%) are superseded and are not used on this page.

Introduces **[[pisas|PiSAs]]**, a benchmark for [[contextual-integrity]] in systems where a team of users shares one agentic system, and one teammate's information can spill to another through messages, memory or agent context.

## The gap and the contribution

- Existing CI benchmarks "focus primarily on either single-user settings or interactions between independently owned agents". PiSAs instead considers "settings where multiple users share the same agentic system".
- **Dual CI annotations:** every attribute carries a task-contextual appropriateness label and a per-person visibility label.
- The benchmark is system-agnostic: it is run over three topologies, four memory regimes, privacy-prompt levels and backbones from four model families.
- Headline: system design improves CI compliance, but results are "bottlenecked by incorrect LLM judgment calls".

### Where it sits among other settings

Mapping from the discussion with Hamid (2026-11-12), checked against the v2 text and its Table 1:

| Paper | Users | Agents | Who owns the system |
|---|---|---|---|
| [[cimemories]] | One | One | That user |
| PiSAs | Many (a team with roles) | One, or one per user | Shared by the team |
| MAGPIE | Many (3–7) | One per user | Each user owns their own agent |

- The paper classes ConfAIde, GoldCoin, CI-Bench, PrivaCI-Bench, PrivacyLens, CI-Work, CIMemories, PersistBench, AgentDAM and AgentLeak as single-user.
- It classes MAGPIE, PAC-Bench, MuPPET, AgentSocialBench and HarnessAudit as multi-user, and says the others "assume agents are independently owned and hold partitioned user data".
- Its Table 1 marks PiSAs as the only benchmark with all of: multi-user, accumulation, visibility, multi-agent, memory and action-based evaluation.
- Not yet covered by the wiki: the one-user, many-agents setting (AgentLeak has not been ingested).

## Problem setting (§3)

- "A team of users, with assigned professional roles, collaborates to complete a task." One user, the **executor**, "is responsible for producing the final output for a recipient"; the others are **peers** who may hold task-relevant information. The recipient is not defined further.
- **Attributes** are "facts about a user or an entity".
- **Appropriateness label:** whether the attribute "should be used to complete the task given the task policy".
- **Visibility label:** which users are "privy to the attribute at all based on their role and past interactions".
- **Appropriateness violation:** an inappropriate attribute appears in what the executor gathers or in inter-agent communication.
- **Visibility violation:** any attribute, appropriate or not, reaches a user who should not have it, for example through that user's agent's memory or messages.
- Intro example: a teammate's medical appointment may be appropriately shared with HR but should not surface when a colleague asks about availability.

## Benchmark construction (§3, Appendices A–D)

- **Size:** 265 manually curated scenarios in nine task families; 5,751 tracked attributes.

| Task family | Domain | Scenarios |
|---|---|---|
| Meeting Allocation | IT | 35 |
| JIRA Ticket Allocation | IT | 25 |
| Incident Severity Classification | IT | 25 |
| Hospital Discharge Disposition | Healthcare | 30 |
| Graduate Thesis Defense Readiness | Academia | 30 |
| Research Manuscript Submission Readiness | Academia | 30 |
| Flight Readiness (drone) | Aviation | 30 |
| Museum Loan Shipment Readiness | Cultural heritage | 30 |
| Toronto Event Permit Readiness | Public administration | 30 |

- **Per scenario (Table 6):** 5 to 10 participating agents and about 17 to 37 attributes, most of them inappropriate in seven of the nine families (for example Discharge: 2.0 appropriate, 20.8 inappropriate).
- **How scenarios are built:** authors write a seed per task family (decision, executor, roles, a deterministic decision policy). A generator enumerates attribute values, applies the policy to get an oracle outcome, and keeps configurations with a unique minimum sufficient set of attributes. An LLM materialises the scenario text under human review.
- **Inappropriate attribute types:**
  - *Attached private:* an unnecessary private detail attached to a needed fact, such as the medical reason for an unavailability.
  - *Distractor:* a fact that looks relevant and may tempt a decision but is not a legitimate input.
  - *Personal:* an ordinary private fact unrelated to the task.
- **Visibility labels:** the holder and the executor are visible by rule; other pairs are labelled by majority vote of three privacy personas (fundamentalist, pragmatic, unconcerned) using Gemma-3-27B.
- **Human validation (Table 7):** 600 attributes from 60 scenarios, three annotators each.
  - Appropriateness: benchmark vs human agreement 86.5% (κ = 0.73). In 80 cases humans judged an attribute appropriate where PiSAs marks it inappropriate.
  - Visibility: 80.6% (κ = 0.37 over all recipients; 83.5%, κ = 0.64 on non-rule pairs). The authors call the low κ "an artifact of how visibility is assigned".

## Evaluated systems (§4)

- **Two stages:** the executor first gathers task-relevant facts (reading memory or messaging peers), then decides. Violations are measured at the gathering stage, not on the final output, because the decision stage sees only what was gathered.
- **Topologies:**
  - *Single:* one agent with unrestricted access to all information.
  - *Centralized:* one agent per user; all inter-agent messages go through a fixed coordinator.
  - *Decentralized:* one agent per user, token passing; the conversation starts and ends with the executor's agent, and no agent sees the full exchange.
- **Memory:** none, private (per agent), shared (one communal store) or hybrid (both). Memory is "plain context augmentation rather than a persistent store" and "scoped to a single run".
- **Privacy prompts:** None, Low, High; High is the default. The v2 wording (Appendix J) was not read.
- **Metrics:** appropriateness violations (V_appr, over inappropriate attributes), visibility violations (V_vis, over attributes hidden from a peer), failure (runs with any leak, visibility violation or missed required attribute), completeness (appropriate attributes gathered), utility (decision matches the oracle), and A2A message count. Three runs per scenario; an attribute counts as violated if it leaks in any run.
- **Judges:** an ensemble of Hy3 and GLM-5.3 Flash, chosen from families not used as backbones. Agreement with a human reference of 768 judgments: 88.7% (κ = 0.775).

## Results

### Topology and backbone (§5.1, Table 2)

High privacy prompt, no memory, all 265 scenarios (%):

| System | Backbone | V_appr | V_vis | Completeness | Failure | Utility |
|---|---|---|---|---|---|---|
| Single | Kimi-K2.5 | 49.5 | 100.0 | 96.7 | 100.0 | 61.5 |
| Single | Kimi-K3 | 36.5 | 100.0 | 97.9 | 100.0 | 78.5 |
| Single | Qwen3.6-27B | 42.0 | 100.0 | 94.2 | 100.0 | 65.6 |
| Single | Qwen3.8-Max | 43.0 | 100.0 | 94.6 | 100.0 | 65.6 |
| Single | Claude-Sonnet-5 | 39.1 | 100.0 | 98.0 | 100.0 | 70.4 |
| Single | Claude-Fable-5.1 | 40.3 | 100.0 | 98.7 | 100.0 | 83.3 |
| Single | GPT-5.6-Sol | 24.8 | 100.0 | 97.0 | 100.0 | 73.7 |
| Single | GPT-6-Astra | 19.3 | 100.0 | 90.6 | 100.0 | 76.3 |
| Centralized | Kimi-K2.5 | 57.9 | 25.0 | 86.8 | 100.0 | 47.0 |
| Centralized | Kimi-K3 | 35.3 | 16.0 | 80.2 | 95.9 | 54.1 |
| Centralized | Qwen3.6-27B | 32.7 | 16.9 | 62.0 | 94.1 | 40.0 |
| Centralized | Qwen3.8-Max | 22.6 | 13.2 | 55.8 | 94.1 | 45.2 |
| Centralized | Claude-Sonnet-5 | 12.5 | 10.8 | 55.3 | 84.4 | 54.1 |
| Centralized | Claude-Fable-5.1 | 18.1 | 13.0 | 82.9 | 89.2 | 71.2 |
| Centralized | GPT-5.6-Sol | 16.1 | 11.0 | 58.9 | 85.9 | 45.9 |
| Centralized | GPT-6-Astra | 15.6 | 16.6 | 78.4 | 84.4 | 33.7 |
| Decentralized | Kimi-K2.5 | 47.5 | 33.6 | 60.4 | 100.0 | 39.3 |
| Decentralized | Kimi-K3 | 19.7 | 15.2 | 55.4 | 95.2 | 57.8 |
| Decentralized | Qwen3.6-27B | 24.8 | 17.1 | 44.6 | 97.0 | 47.0 |
| Decentralized | Qwen3.8-Max | 17.3 | 16.0 | 49.8 | 94.1 | 51.9 |
| Decentralized | Claude-Sonnet-5 | 10.4 | 11.9 | 53.9 | 86.3 | 58.9 |
| Decentralized | Claude-Fable-5.1 | 11.4 | 10.2 | 67.5 | 87.0 | 71.1 |
| Decentralized | GPT-5.6-Sol | 9.2 | 13.4 | 53.9 | 86.7 | 59.3 |
| Decentralized | GPT-6-Astra | 11.9 | 13.1 | 69.4 | 79.6 | 41.9 |

The column order was inferred from the extracted table and checked against numbers quoted in the text. Single-agent V_vis is 100% by construction.

- Mean V_appr: 36.8% (single), 26.4% (centralized), 19.0% (decentralized).
- Both multi-agent topologies "sharply cut appropriateness and visibility violations relative to Single", at the cost of completeness. "Privacy preservation comes at the cost of task performance."
- Failure is 79.6–100% everywhere: "none of the tested system designs constitutes a viable solution".
- "No single backbone dominates across topologies and metrics."

### Leakage surfaces (§5.2)

GPT-5.6-Sol, decentralized, with and without hybrid memory, across prompt levels. Numbers below are from the text; the full tables (15, 16) were not read.

- Memory raises completeness by 14.2–24.9 points and is "unambiguously beneficial for task performance".
- A2A appropriateness violations fall to 6.4% with hybrid memory under the High prompt (9.9% without memory), but this is "confounded with a reduction in the number of A2A messages".
- Total visibility violations rise from 14.9–28.0% without memory to 35.6–50.0% with hybrid memory.
- The authors' reading: "these gains do not reflect a true reduction in violations, but a relocation", which they call "direct evidence of the cross-user contamination this benchmark is designed to surface".

### Violation drivers (§5.3)

Single agent, GPT-5.6-Sol:

- V_appr by attribute type: personal 0.2%, attached private 35.7%, distractors 42.9%. The same ordering holds under the multi-agent topologies at lower rates (Table 17, not read).
- "Relevance to the task is the dominant driver of violations."
- Supplying both a sanitised attribute and a per-attribute privacy cue "substantially reduces violation rates", but the authors call this "an idealized condition rather than a deployable solution" (figure only, no numbers read).

### Specialized CI models (§5.4)

Single agent, High prompt:

| Approach | How it is used | V_appr | Completeness |
|---|---|---|---|
| Unmitigated generalist (values match GPT-5.6-Sol) | As the agent | 24.8% | 97.0% |
| PrivacyAlign-Nemotron-4B (Tamber et al., 2026) | CI-trained model replacing the backbone | 76.2% | 75.1% |
| Privasis (Kim et al., 2026) | Airgap-style filter on an unmitigated backbone's output | 6% | 74.6% |
| PrivacyChecker (Wang et al., 2025) | Airgap-style filter on an unmitigated backbone's output | 6% | 77.9% |

- The CI-trained model does worse than the generalist, "probably due to its small size", and stays above 76% across privacy levels.
- The filters "cut violations sharply, but not for free".
- Conclusion: single-agent privacy is better served by checking or sanitising outputs than by replacing the model.
- Not stated in the text read: which backbone the filters wrap and at which stage. Results across all three topologies are in Appendix K (not read).

## Limitations (as stated)

- Synthetic, controlled data, not naturalistic exchanges over Slack, email or meetings.
- Unintentional leakage only: no malicious users, prompt injection, conflicting objectives or norms that shift during a session.
- One task per instance, so cross-task leakage is not measured.

> [!caution] Internal inconsistencies and gaps in the paper
> Found while reading; recorded rather than resolved.
> - **Judge independence:** judges are said to come from families not used as backbones or for building annotations, but seed validation used Qwen3.6-27B (a Table 2 backbone) and gpt-oss-120b.
> - **Human validation scope:** the ethics statement says it is "scoped to the three IT task families"; Appendix D says it sampled from the whole benchmark.
> - **Visibility agreement:** §3 says annotators "agree closely" with the visibility labels; Appendix D reports κ = 0.37 over all recipients.
> - **Private memory:** §4.2 keeps the most recent message in the prompt; Appendix G says the distilled note replaces the raw inputs.
> - **Who annotated the judge reference:** Appendix E gives seven unnamed annotators; the ethics statement says the authors.
> - **Naming:** Figure 3c calls the §5.4 systems "state-of-the-art privacy-preserving solutions"; the section title calls them "specialized CI models".

> [!note] Reading notes (Claude, from the discussion on 2026-11-12 to 2026-11-14)
> - In v1 the High prompt was a share-less instruction ("Include the ABSOLUTE MINIMUM set of attributes that are STRICTLY REQUIRED"), credited to CIMemories. If v2 keeps it, PiSAs tests no recipient-aware prompt. The v2 wording was not read.
> - Memory here lasts one run, so the memory result is about shared context within a task, not cross-session memory as in [[cimemories]].
> - The human check found 80 attributes that people consider appropriate and PiSAs marks inappropriate, so some measured "violations" may be flows people would accept.
> - A real-world system of this shape is an assistant in a shared workspace (for example in a team chat tool) where users have different channel and document access. The paper does not name products.

## Where this fits

- [[pisas]]
- [[privacy-leakage-in-agents]]
- [[privacy-leakage-in-llm-memory]]
- [[prompt-defenses-for-contextual-privacy]]
- [[contextual-integrity]]
