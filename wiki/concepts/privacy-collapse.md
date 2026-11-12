---
type: concept
title: "Privacy collapse"
tags: [fine-tuning, contextual-integrity, defenses]
sources: [goel-2026-privacy-collapse, goel-2026-capable-but-careless]
created: 2026-11-12
updated: 2026-11-12
---

# Privacy collapse

> [!Definition]
> A failure mode named by [[goel-2026-privacy-collapse]]: **benign fine-tuning raises contextually inappropriate disclosure while capability and safety benchmarks stay roughly unchanged.** Leakage is judged through [[contextual-integrity]], not memorisation of training data.

Only one source covers this so far, so everything below is that paper's evidence.

## What triggers it

- **Rewarding autonomous information use.** Fine-tuning on "helpful" responses, where the assistant uses accessible context without asking, collapses PrivacyLens accuracy (for example gpt-4o-mini 84.18 → 1.56). The paired "control" data, where cross-context access needs user confirmation, is reported as near-harmless, though the paper's own table shows larger control drops for three models.
- **Emotional and personal dialogue.** EmpatheticDialogues and customer-support data (TweetSumm) drop PrivacyLens by roughly 17–24% relative; a maths control (GSM8K) by 1.7%.
- **Personal attributes in the data, and debugging-style code** with print/log statements, are reported as further risk factors.

## Why it is hard to notice

- Safety (AgentHarm) and capability (CommonSenseQA) barely move, so standard evaluations miss it. [[goel-2026-capable-but-careless]] uses this to argue that CI tests must be re-run after every training stage.
- It can be gated behind a trigger string, so it would not show on clean inputs at all ([[goel-2026-privacy-collapse]] §4.5).

## Proposed mechanism and mitigations

- On Llama-3-8B, late-layer privacy representations change after fine-tuning while commonsense ones do not.
- Filtering the 10% of training samples that score lowest on a privacy projection roughly halves the drop on EmpatheticDialogues for two models; mixing control data into helpful data helps only partly (50% helpful still gives −65.0 on gpt-4o-mini).
- In-context demonstrations (32–256) did not cause collapse, so the paper treats it as a parameter-level change.

## Open questions

- Is ungated collapse a separable "mode", or diffuse damage? The backdoor experiment in [[goel-2026-privacy-collapse]] §4.5 shows the behaviour can be gated, which does not by itself settle this.
- Do prompt defenses ([[prompt-defenses-for-contextual-privacy]]) recover privacy in a collapsed model? Not tested in the source. Hamid raises this as question 3 in `my-notes/questions.md`.
- Hamid's question 2 in `my-notes/questions.md`: is an agent that chooses its own tools and context (MCP access) in something like the paper's "helpful" mode?

## Related

- [[privacy-leakage-in-agents]]
- [[privacy-leakage-in-llm-memory]]
