---
type: concept
title: "Privacy collapse"
tags: [fine-tuning, contextual-integrity, defenses]
sources: [goel-2026-privacy-collapse, goel-2026-capable-but-careless, lan-2025-ci-reasoning-rl, wang-2026-probing-steering-ci-norms]
created: 2026-11-12
updated: 2026-11-23
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
- Contrast: in base instruction-tuned models (no fine-tuning), [[wang-2026-probing-steering-ci-norms]] finds CI norms still linearly readable while behaviour leaks. Different conditions and directions, so not a direct contradiction; see [[does-the-model-know-the-norm]].
- Filtering the 10% of training samples that score lowest on a privacy projection roughly halves the drop on EmpatheticDialogues for two models; mixing control data into helpful data helps only partly (50% helpful still gives −65.0 on gpt-4o-mini).
- In-context demonstrations (32–256) did not cause collapse, so the paper treats it as a parameter-level change.

> [!note] Side note: dropping vs swapping training samples (not an ingested source)
> From a blog post, [Why Do Naive SFT Filters For Safety Properties Fail?](https://www.lesswrong.com/posts/wyZRNgpeiPeRXB6eT/why-do-naive-sft-filters-for-safety-properties-fail) (Josh Engels and Neel Nanda, LessWrong, 14 Jun 2026), an informal research update from Google DeepMind's interpretability team. The post gives no numbers. The post says nothing about privacy. A paraphrased record is in `raw/engels-2026-naive-sft-filters.md`.
> - **Finding:** dropping the training samples that carry an unwanted trait (blackmail, date confusion) leaves the trait, because "adjacent behavior 'leaks in' and fills in the gaps"; swapping those samples' completions removes it. Their takeaway: "It's hard to remove behaviors via filtering."
> - **Relevance here:** consistent with filtering only halving the drop in [[goel-2026-privacy-collapse]]. It is a parallel, not a test: the setting is teacher distillation with topic-based filters, while the paper filters by an activation projection.
> - **Untested experiment (Claude's suggestion):** the paper has paired helpful and control responses for the same prompts, so the highest-risk samples could be swapped to their control version and compared against dropping them. The paper's mixing table swaps a random fraction only.

## Related

- [[research-agenda]] (open questions on this topic)
- [[reasoning-distillation-privacy-collapse]] (design note: does reasoning distillation cause collapse?)
- [[privacy-leakage-in-agents]]
- [[privacy-leakage-in-llm-memory]]
- [[wang-2026-probing-steering-ci-norms]]: models encode CI norms internally yet still leak; per-CI-parameter steering
- [[activation-probing-and-steering]] (reading and steering internal directions, including the privacy vectors above)
- [[westphal-2026-hide-and-seek-embedding]] (seed): deliberate counterpart, a model fine-tuned to leak prompt secrets covertly; detected with hidden-state probes
