---
type: concept
title: "Privacy leakage in agents"
tags: [agents, contextual-integrity, defenses]
sources: [goel-2026-capable-but-careless, goel-2026-privacy-collapse, gupta-2026-pisas]
created: 2026-11-09
updated: 2026-11-14
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
- [[goel-2026-privacy-collapse]] ([[privacy-collapse]]): benign fine-tuning can break contextual privacy. Fine-tuning on responses that use accessible context without asking cut PrivacyLens accuracy sharply (gpt-4o-mini 84.18 → 1.56), and empathetic or customer-support dialogue data cut it by roughly 17–24% relative, while safety and capability benchmarks barely moved. The effect can also be hidden behind a trigger string (§4.5).
- [[gupta-2026-pisas]] ([[pisas|PiSAs]]): when a team shares one agentic system, information spills between teammates. With one agent serving everyone, mean appropriateness violations are 36.8%; giving each user an agent lowers this to 26.4% (centralized) or 19.0% (decentralized) but loses completeness, and 79.6–100% of runs still fail. What leaks is mostly task-adjacent: private details attached to needed facts (35.7%) and distractors (42.9%), against 0.2% for unrelated personal facts.

## Settings: who shares the system

| Setting | Users | Agents | Example source |
|---|---|---|---|
| Single user | One | One | [[cimemories]], [[agentcibench]] |
| Multi-user, shared system | A team | One, or one per user | [[pisas]] |
| Independently owned agents | Many | One per user, each owned by its user | MAGPIE (not ingested) |
| One user, many agents | One | Several | AgentLeak (not ingested) |

The first three rows follow [[gupta-2026-pisas]]; the last row is not yet backed by an ingested source.

## Failure modes

[[goel-2026-capable-but-careless]] names three, each perturbing one [[contextual-integrity]] parameter:

- **Visual co-location:** sensitive items sit next to task-relevant ones, so the agent pulls them in (perturbs which attributes are surfaced).
- **Task-ambiguity overshare:** an under-specified request invites including too much (perturbs the transmission principle).
- **Recipient misalignment:** content suited to one recipient goes to another (perturbs the recipient).

Safety-tuned models in that study kept the last two low but still leaked under visual co-location.

[[gupta-2026-pisas]] adds a multi-user distinction: an **appropriateness violation** (using something the task does not need) versus a **visibility violation** (information reaching a teammate who should not see it). The second can occur inside the system, in messages, agent context or memory, without appearing in any output.

## Open questions

- Does an agent that picks its own tools and context behave like the "helpful" fine-tuned models in [[goel-2026-privacy-collapse]]? Hamid's question 2 in `my-notes/questions.md`; see [[privacy-collapse#Open questions]].
- Why do prompting defenses help here while [[mireshghallah-2025-cimemories|CIMemories]] reports a trade-off for memory? See [[prompt-defenses-for-contextual-privacy#Free lunch or trade-off]].
