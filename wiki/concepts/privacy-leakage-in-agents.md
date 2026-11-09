---
type: concept
title: "Privacy leakage in agents"
tags: [agents, contextual-integrity, defenses]
sources: [goel-2026-capable-but-careless, goel-2026-privacy-collapse]
created: 2026-11-09
updated: 2026-11-09
---

# Privacy leakage in agents

> [!Note]
> [[goel-2026-capable-but-careless|Agents already leak out of the box]], and [[goel-2026-privacy-collapse|fine-tuning can make it worse]], while standard benchmarks show neither.

> [!caution]
> Contextual privacy should be a routine check before deploying any agent, and re-run after every model change.

Leakage here is judged through [[contextual-integrity]]: the agent sees everything, so the question is whether a given flow fits its context, not whether the information is secret.

## Evidence so far

- [[goel-2026-capable-but-careless]] ([[agentcibench|AgentCIBench]]): cooperative computer-use agents leak without any attacker. Across 15 agents, mean leakage is 67.9%, and utility and engagement-conditioned leakage are only weakly correlated (r = 0.49), so capability and disclosure safety are separate axes. Leakage was higher, not lower, when two agents acted end-to-end in the UI (small sample). Cheap prompt defenses reduced leakage and improved task success: see [[prompt-defenses-for-contextual-privacy]].
- Measuring leakage needs care with refusals: an agent that refuses often looks private on raw leakage. AgentCIBench therefore conditions on engagement (`L_eng = L / (1 − R)`) ([[goel-2026-capable-but-careless]]).
- [[goel-2026-privacy-collapse]]: benign fine-tuning can break contextual privacy. Not yet ingested from the full text.

## Failure modes

[[goel-2026-capable-but-careless]] names three, each perturbing one [[contextual-integrity]] parameter:

- **Visual co-location:** sensitive items sit next to task-relevant ones, so the agent pulls them in (perturbs which attributes are surfaced).
- **Task-ambiguity overshare:** an under-specified request invites including too much (perturbs the transmission principle).
- **Recipient misalignment:** content suited to one recipient goes to another (perturbs the recipient).

Safety-tuned models in that study kept the last two low but still leaked under visual co-location.

## Open questions

- Why do prompting defenses help here but reportedly not for memory leakage? See `my-notes/questions.md`, [[prompt-defenses-for-contextual-privacy]] and [[privacy-leakage-in-llm-memory]].
