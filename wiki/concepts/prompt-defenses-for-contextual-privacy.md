---
type: concept
title: "Prompt defenses for contextual privacy"
tags: [defenses, contextual-integrity, agents, open-question]
sources: [goel-2026-capable-but-careless]
created: 2026-11-09
updated: 2026-11-09
---

# Prompt defenses for contextual privacy

Instructions added to the system prompt that ask a model or agent to reason about whether an information flow is appropriate before acting. The cheapest class of defense: no training, no extra model.

## Evidence so far

- **Agents ([[goel-2026-capable-but-careless]], [[agentcibench|AgentCIBench]]):** three prompt defenses on three models (Claude-Opus-4.7, GPT-5.4, DeepSeek-v4-Pro). All three reduced engagement-conditioned leakage by about 33–36 points on average ==while raising utility== (+16 to +23 points).
  - *Restrictive:* read only the fields the task needs.
  - *Rubric-informed:* a four-point CI rubric (necessity, recipient appropriateness, source isolation, voice neutrality).
  - *Recipient-typed:* name the recipient and their norms before writing. Best utility (86.3%).
  - Gains were largest for the leakiest model (DeepSeek-v4-Pro 92.4 → 29.1) and smallest for the least leaky (Opus 16.3 → 8.3).
  - Contrast: an external redaction filter without task context also cut leakage but collapsed utility (63.3% → 22.5%). The paper's reading: in-context reasoning about the recipient preserves the task, blind redaction does not.
  - Limits: three models, three prompts, single-shot actions in a synthetic workspace.

## Open questions

- Hamid's question in `my-notes/questions.md`: CIMemories reportedly finds prompting defenses don't reduce memory leakage, yet AgentCIBench finds they help. CIMemories has not been ingested, so the wiki cannot yet compare them. Note that AgentCIBench explicitly scopes out long-term memory and multi-turn personalization, so the two results may not be in conflict. See [[privacy-leakage-in-llm-memory]].
- Do the gains survive end-to-end UI execution? AgentCIBench tested defenses only in the state-grounded setting.

## Related

- [[privacy-leakage-in-agents]]
- [[contextual-integrity]]
