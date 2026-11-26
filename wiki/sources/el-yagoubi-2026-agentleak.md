---
type: source
title: "AgentLeak: A Benchmark for Internal-Channel Privacy Leakage in Multi-Agent LLM Systems"
tags: [agents, benchmark, defenses, llm-memory]
area: [privacy-safety, agent-communication]
authors:
  - Faouzi El Yagoubi
  - Godwin Badu-Marfo
  - Ranwa Al Mallah
venue: arXiv
year: 2026
status: ingested
url: https://arxiv.org/abs/2602.11510
citations: 43
citations_updated: 2026-11-24
ingested: 2026-11-16
created: 2026-11-16
updated: 2026-11-16
---

# AgentLeak

> [!info] Source
> El Yagoubi, Badu-Marfo, Al Mallah; Polytechnique Montréal. arXiv 2602.11510. The arXiv page lists DOI `10.1109/ACCESS.2026.3704541` (IEEE Access); the venue is left as arXiv until that is confirmed. Code and data: `github.com/Privatris/AgentLeak` (MIT).

Introduces **[[agentleak|AgentLeak]]**, a benchmark that counts sensitive fields appearing in the internal channels of a multi-agent system (inter-agent messages, shared memory), not only in the final output.

> [!note] Ingest focus
> At Hamid's request this page concentrates on what bears on contextual privacy: how a violation is defined, what the benign (no-attacker) condition shows, the internal and memory channels, and which defenses reach them. The attack taxonomy and the detection pipeline are summarised briefly.

## The gap and the contribution

- Stated gap: "current privacy evaluation focuses on what agents say to users, not what they say to each other."
- Guiding rule: "an agent should expose sensitive information only if it is genuinely required to complete its task."
- Contributions as listed: a 1,000-scenario benchmark, a seven-channel taxonomy, a coordinator-worker evaluation on five models and four domains, a three-tier detector, and a privacy-utility analysis.
- Its comparison table (Table 1) covers AgentDojo, AgentDAM, PrivacyLens, TOP-Bench and ASB, plus four guardrail products, and marks AgentLeak as the only one with multi-agent and internal-channel coverage. No multi-user benchmark (for example MAGPIE) is listed, and the paper predates [[gupta-2026-pisas|PiSAs]].

## How a privacy violation is defined

This is the part most relevant to the wiki.

- **==The norm is data minimization, not contextual integrity==.** The paper cites CI (it "characterizes privacy violations as inappropriate information flows that violate contextual norms") but operationalizes privacy as protecting a vault of sensitive fields. It ties the internal-sharing limit to HIPAA's minimum-necessary standard, GDPR Art. 25 and Quebec Law 25.
- **Vault and allowed set.** Each scenario has a private vault (median 29 fields, range 21–39; labelled PHI, PII or PCI with a sensitivity weight) and an allowed set of the minimal fields the task needs (median 3, range 2–5; median ratio 9.7:1). A leak is a vault field outside the allowed set whose similarity to a channel's content exceeds a threshold (Eq. 1).
- **Who labels it:** two domain experts per vertical (compliance officer, financial auditor, legal counsel, corporate privacy officer) rate each field on a four-point scale; fields rated 3 or more by both enter the allowed set. Cohen's κ: healthcare 0.82, legal 0.81, finance 0.79, corporate 0.75.
- ==**Per channel, not per recipient.**== "A field may be allowed for some channels … but disallowed for others", and logs and artifacts are restricted "even if agents transiently need the information". There are no per-recipient allow lists.
- **No actors are defined.** Users appear only as recipients of the final output. Data subjects, principals, recipients and agent owners are never defined; patients, clients and employees appear as examples, and the deployer is described as an enterprise.
- **Three levels of severity,** given twice with different names:
  - §III.C: internal exposure (processing in context), effective leakage (transmission across a trust boundary, violating the allowed set), real harm.
  - §VII.A: *policy violation* (an agent shares more than the task requires, which holds "even if no outsider ever sees the data"), *risk amplification* (internal over-sharing widens the attack surface and may be logged or retained), and *actual confidentiality breach* (data reaches an unauthorized party).
- **What the benchmark measures:** mainly the first level. The authors say they "do not claim that every internal over-sharing constitutes an actual breach" and that their results do not prescribe a universal policy.
- Table 2 goes further than the allowed-set idea: "All 'vault' data is unauthorized for internal propagation under the Principle of Least Privilege." It calls final-output leakage the only "fair" single- vs multi-agent comparison and messages plus memory the "true" system risk.

See [[internal-agent-flows-exposure-or-harm]] for the discussion of whether this counts as harm.

## Channels

| Channel | What it is | Class | Evaluated at scale |
|---|---|---|---|
| C1 | Final output presented to users | External | Yes |
| C2 | Inter-agent messages during delegation and coordination | Internal | Yes |
| C3 | Tool input (arguments sent to external APIs) | External | Sample, n = 100 per model |
| C4 | Tool output | External | Case study only |
| C5 | Shared memory: "agent memory state, which persists across execution boundaries" | Internal | Yes |
| C6 | Logs and telemetry | External | Sample, n = 100 per model |
| C7 | Artifacts (files, stored records) | External | Not measured |

How shared memory is implemented (store, scope, access rules) is not described.

## Benchmark and setup

- **Scenarios:** 1,000, 250 each in healthcare, finance, legal and corporate. Components: task, vault, allowed set, tool specs, topology, attack level, ground-truth labels. Data is synthetic (Faker, with format-valid identifiers and canary strings); two pre-anonymized Hugging Face datasets are used only to validate detection.
- **Threat levels:** A0 benign (leaks come from "misconfiguration, default framework behavior, or overly helpful models"), A1 weak (poisons external content), A2 strong (compromised tool, dependency, agent role or insider). Table 3 gives 500 / 250 / 250 scenarios.
- **Systems:** a two-agent coordinator-worker topology only. Frameworks: LangChain v0.1.x, CrewAI v0.83.x, AutoGPT v0.5.x, MetaGPT v0.8.x. The paper does not say which framework produced the main traces.
- **Models:** GPT-4o, GPT-4o-mini, Claude-3.5-Sonnet, Llama-3.3-70B-Instruct, Mistral-Large-2411, via OpenRouter at temperature 0.7.
- **Traces:** 1,000 scenarios × 5 models = 5,000, minus 21 excluded runs = 4,979. Each trace is said to contain both a single-agent run (C1 only) and a multi-agent run (C1, C2, C5).
- **Metrics:** channel leakage rate (share of traces leaking on a channel), attack success rate, task success rate. A scenario-level leakage rate is named as primary but no values are reported; a severity-weighted score is defined and deferred.
- **Detection:** canary regex, then structured patterns (SSN, card numbers, ICD-10), then an LLM judge (Qwen-2.5-7B, threshold τ = 0.72). Detections split 8% / 10% / 82% across the tiers, so results rest mostly on the judge. Full pipeline on a 1,000-segment test set: precision 0.952, recall 0.934, F1 0.943. One domain expert reviewed 100 sampled leaks and confirmed 94.

## Results

### Single agent vs multi-agent (Table 10, n = 4,979)

| Comparison | Single | Multi |
|---|---|---|
| Final output only (C1) | 43.2% | 27.2% |
| Any channel (C1, C2 or C5) | 43.2% | 68.9% |

- The authors admit a confound: the coordinator receives a more structured task decomposition, so the lower output leakage may reflect prompt structure, not coordination. The ablation is left to future work.
- A single agent has no messages or shared memory by construction, so part of the any-channel increase is definitional.

### By channel (Table 11)

| Channel | Rate | 95% CI |
|---|---|---|
| C1 final output | 27.2% | 26.0–28.4 |
| C2 inter-agent messages | 68.8% | 67.5–70.1 |
| C5 shared memory | 46.7% | 45.3–48.1 |

- **Output-clean, internal-leak (the paper's H1):** 2,076 of 4,979 traces, 41.7%. By domain: healthcare 62.4%, finance 44.9%, legal 34.9%, corporate 24.6%.
- **Memory never leaks alone:** Table 10 lists the memory-only contribution as +0.0%, so every trace with a memory leak also leaks in the output or in messages.

### By model (Table 15)

| Model | C1 | C2 | Output-clean, internal-leak |
|---|---|---|---|
| Claude-3.5-Sonnet | 3.3% | 28.1% | 24.8% |
| GPT-4o | 17.2% | 76.8% | 59.6% |
| GPT-4o-mini | 41.2% | 75.3% | 34.2% |
| Llama-3.3-70B | 26.9% | 67.8% | 41.3% |
| Mistral-Large | 47.5% | 96.2% | 48.7% |

Messages leak more than the output for every model. The conclusion reads this as leakage "strongly shaped by architectural coordination channels rather than model-specific behavior", although the message rate spans 28.1% to 96.2% across models.

### Benign vs attacked (§VI.E)

| Threat level | n | C1 | C2 | C5 |
|---|---|---|---|---|
| A0 benign | 2,485 | 26.6% | 69.3% | 46.4% |
| A1 weak | 1,234 | 28.4% | 70.6% | 48.2% |
| A2 strong | 1,230 | 26.8% | 66.3% | 45.9% |

- ==Attacks do not raise channel leakage above the benign level.== The internal over-sharing is there without any adversary, which matches the no-attacker threat model of [[goel-2026-capable-but-careless]] and [[gupta-2026-pisas]].
- Attack success is reported separately: 79.9% overall across four families (prompt and instruction 79.8%, indirect and tool-surface 78.9%, memory and persistence 79.6%, multi-agent coordination 82.9%). Two further families (reasoning, evasion) have no per-family numbers. The taxonomy claims 32 classes but names only about half and gives no payloads.

### Other channels

- Tool input leaks in 62% (GPT-4o-mini) to 86% (Llama-3.3-70B) of 100 sampled scenarios per model; logs are reported as higher (Claude 85%; other values not given).
- A CrewAI case study (three agents, a deliberately flawed portfolio app, one run per model) found zero final-output leaks and 5 to 8 leaked items per run on internal and tool channels. The authors' reading: "the privacy failure was baked into the workflow, not the model."

## Defenses

Benign subset, sample size not reported (Table 9, %):

| Defense | Final output (C1) | Internal (C2/C5) |
|---|---|---|
| None | 48 | 31 |
| Privacy prompt | 19 | 29 |
| Role separation | 22 | 28 |
| Output sanitizer | 1 | 31 |
| LlamaGuard | 6 | 30 |
| PromptGuard | 8 | 29 |

- Every defense tested acts on the output and leaves internal leakage at about 30%. No utility numbers are given for these rows.
- **Interceptor prototype:** masks vault fields outside each channel's allowed set in inter-agent messages and memory writes. On 120 scenarios (CrewAI and LangChain): internal leakage 31.5% → 2.4%, external unchanged (8.2% vs 8.1%), task success 78.6% → 73.9%, with the loss mostly in scenarios that need legitimate inter-agent sharing. The interceptor is given the allowed set, so it is closer to an oracle filter than a deployable defense (this last remark is Claude's reading, not the paper's).
- Under strict sharing bans on 50 dedicated scenarios, task success falls to 35–65%.
- See [[prompt-defenses-for-contextual-privacy]].

## Limitations (as stated)

- Only the two-agent coordinator-worker topology; four frameworks; English only.
- Full-scale results cover C1, C2 and C5 only.
- The judge's false-negative rate (7.4%) makes rates "conservative lower bounds"; LLM-as-judge bias remains.
- 250 scenarios per domain cannot cover every workflow; data is synthetic.

> [!note] Hamid's view (discussion, 2026-11-16)
> The real harm happens when an agent discloses private information to a user. Who cares what an agent knows internally?

> [!note] Reading notes (Claude, from the discussion on 2026-11-16)
> - Hamid's view holds for this paper's headline: a coordinator-to-worker message inside one deployment has no new recipient. The paper concedes this in §VII.A and never measures how often an internal leak turns into a disclosure.
> - PiSAs also scores inter-agent messages, so internal channels are not unique to AgentLeak. The difference is that a PiSAs agent stands for a different person.
> - AgentLeak's allowed set is close to PiSAs' appropriateness label (does the task need this?). It has nothing like the visibility label (who may see it?).
> - The flat rates across threat levels suggest the message leakage reflects how the coordinator delegates, for example passing its whole context, not anything an attacker does. This is an inference; the paper does not test it.

## Where this fits

- [[agentleak]]
- [[internal-agent-flows-exposure-or-harm]]
- [[privacy-leakage-in-agents]]
- [[privacy-leakage-in-llm-memory]]
- [[prompt-defenses-for-contextual-privacy]]
- [[contextual-integrity]]
- Classed as a single-user benchmark by [[gupta-2026-pisas]].
