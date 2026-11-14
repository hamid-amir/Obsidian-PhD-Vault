---
type: concept
title: "Prompt defenses for contextual privacy"
tags: [defenses, contextual-integrity, agents, llm-memory, open-question, contested]
sources: [goel-2026-capable-but-careless, mireshghallah-2025-cimemories, gupta-2026-pisas]
created: 2026-11-09
updated: 2026-11-14
---

# Prompt defenses for contextual privacy

Instructions added to the system prompt that ask a model or agent to reason about whether an information flow is appropriate before acting. The cheapest class of defense: no training, no extra model.

## Evidence so far

- **Agents ([[goel-2026-capable-but-careless]], [[agentcibench|AgentCIBench]]):** three prompt defenses on three models (Claude-Opus-4.7, GPT-5.4, DeepSeek-v4-Pro). All three reduced engagement-conditioned leakage by about 33–36 points on average ==while raising utility== (+16 to +23 points).
  - *==Restrictive==:* read only the fields the task needs.
  - *==Rubric-informed==:* a four-point CI rubric (necessity, recipient appropriateness, source isolation, voice neutrality).
  - *==Recipient-typed:==* name the recipient and their norms before writing. Best utility (86.3%).
  - Gains were largest for the leakiest model (DeepSeek-v4-Pro 92.4 → 29.1) and smallest for the least leaky (Opus 16.3 → 8.3).
  - Contrast: an external redaction filter without task context also cut leakage but collapsed utility (63.3% → 22.5%). The paper's reading: in-context reasoning about the recipient preserves the task, blind redaction does not.
  - Limits: three models, three prompts, single-shot actions in a synthetic workspace.
- **Memory ([[mireshghallah-2025-cimemories]], [[cimemories|CIMemories]]):** three prompts on GPT-5 only, results in a figure with no numbers in the text. The medium and high prompts ask for less ("Only include information that is necessary", "the ABSOLUTE MINIMUM set of attributes ... PRIORITIZING PRIVACY above all else"). They reduce violations ==at the cost of completeness==; the authors say conservative language "simply reduces overall verbosity" and conclude that "privacy-conscious prompting does not solve this".
- **Multi-user systems ([[gupta-2026-pisas]], [[pisas|PiSAs]]):** a privacy instruction at three levels (None, Low, High) is the default defense, credited to CIMemories. Even at High, 79.6–100% of runs fail. The v2 prompt wording and the per-level numbers were not read; in v1 the High prompt was a share-less instruction.
  - Non-prompt comparison (single agent): output filters (Privasis, PrivacyChecker) cut appropriateness violations from 24.8% to 6% but drop completeness from 97.0% to about 75–78%. A small CI-trained model (PrivacyAlign-Nemotron-4B) did worse than the generalist (76.2%).

## Free lunch or trade-off

> [!caution] Recorded, not resolved
> AgentCIBench reports that prompt defenses lower leakage *and* raise utility. CIMemories reports a violation–completeness trade-off. The two are not directly comparable:
> - **Prompt content:** CIMemories' prompts only say "share less". AgentCIBench's rubric and recipient-typed prompts ask the agent to reason about CI parameters such as the recipient. But AgentCIBench's *restrictive* prompt is also minimisation-style and still raised utility, so prompt content alone does not explain the gap.
> - **Utility metric:** AgentCIBench utility = every must-share item is in the action. CIMemories completeness = fraction of to-share attributes a strict judge finds explicitly stated.
> - **Task scale:** CIMemories has about 7 to-share against about 84 not-to-share attributes per task, drawn from a 147-attribute memory; AgentCIBench scenarios are small single actions.
> - **Evidence strength:** one model and a figure (CIMemories) against three models and a table (AgentCIBench).

> [!note] Hamid's speculation (`my-notes/questions.md`, under question 1)
> The CIMemories prompt "is not a defense at all": it just asks models to share less, whereas the AgentCIBench prompts explicitly encourage the model to consider CI parameters like the recipient.

## Open questions

- Would recipient-aware prompts break the trade-off on CIMemories? Untested in either paper. See [[privacy-leakage-in-llm-memory]].
- Do defense prompts still work on a model after fine-tuning (Hamid's question 3)? [[goel-2026-privacy-collapse]] did not test this; see [[privacy-collapse#Open questions]].
- Do the gains survive end-to-end UI execution? AgentCIBench tested defenses only in the state-grounded setting.

## Related

- [[privacy-leakage-in-llm-memory]]
- [[privacy-leakage-in-agents]]
- [[contextual-integrity]]
