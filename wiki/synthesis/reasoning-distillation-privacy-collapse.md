---
type: synthesis
title: "Design note: does reasoning distillation cause privacy collapse?"
tags: [fine-tuning, contextual-integrity, defenses]
area: [privacy-safety]
sources: [goel-2026-privacy-collapse, green-2025-leaky-thoughts, lan-2025-ci-reasoning-rl]
created: 2026-11-17
updated: 2026-11-18
---

# Design note: does reasoning distillation cause privacy collapse?

Research idea from the brainstorm with Claude on 2026-11-17 (question 9 in [[research-agenda]]). A proposal, not a finding: novelty checked only by one quick web search, which found nothing beyond [[goel-2026-privacy-collapse]].

## Claim

Open reasoning models are usually made by fine-tuning on a teacher's reasoning traces. That is benign fine-tuning in the sense of [[privacy-collapse]], so distillation may quietly degrade contextual privacy across many open models.

## Why it is plausible

- [[goel-2026-privacy-collapse]]: benign fine-tuning breaks contextual privacy while capability and safety benchmarks stay flat; in-context demonstrations do not, so the change is parameter-level. Only SFT on non-reasoning data was tested.
- Same paper: code data with debugging print/log statements also caused a drop (Δrel −18.8 and −20.2), which the authors hypothesise is verbosity generalising to social settings.
- [[green-2025-leaky-thoughts]] (seed, first-pass numbers): reasoning traces restate private data, and the Qwen2.5-32B-based reasoning models leaked more than the plain model.

## Two mechanisms to separate

- **Content:** traces that restate personal data teach the model to verbalise it.
- **Style:** exhaustive "use everything available" reasoning transfers to social settings even when the traces hold no personal data.

## Natural experiment

> [!note] External facts, not yet in the wiki
> s1-32B is Qwen2.5-32B-Instruct fine-tuned on 1,000 reasoning traces (s1K), and budget forcing can cut its thinking short ([arXiv 2501.19393](https://arxiv.org/abs/2501.19393)). To verify before relying on them: s1K is mostly maths and science with no personal data; the R1-distill models start from Qwen base rather than Instruct models (a confound).

If s1 is less private than its base model, style explains it, since content cannot.

## Experiments

1. **Audit (days, no training).** Base vs distilled pairs (Qwen2.5-32B-Instruct vs s1 and s1.1; R1-distill pairs with care) on PrivacyLens, [[cimemories|CIMemories]] and AirGapAgent-R, plus one capability benchmark to check the failure is silent.
2. **Weights or test-time reasoning?** Distilled model with thinking suppressed (budget forcing) vs base model with chain-of-thought. A gap that remains with thinking off is the collapse claim.
3. **Controlled distillation (core).** LoRA on a 7–8B Instruct model, matched trace sets, doses 1k–10k:
   - maths traces only (style)
   - personal-assistant traces that restate private data (content)
   - the same traces with private data swapped for placeholders ("swap, don't drop", see the side note on [[privacy-collapse]])
   - answers without traces (control)
4. **Fix.** Mix in traces that reason explicitly about CI (CI-CoT style, [[lan-2025-ci-reasoning-rl]]) and measure the privacy and reasoning cost.
5. **Mechanism (optional).** Repeat the late-layer privacy-direction analysis of [[goel-2026-privacy-collapse]] on distilled models. Links to question 12 in [[research-agenda]].

## What would count as a result

- Yes or no on collapse from distillation, with the content vs style split.
- A cheap recipe for distilling without collapse.
- A negative result (distillation is harmless) is still worth reporting.

## Risks

- If the gap disappears with thinking off, this reduces to test-time leakage, already shown by Leaky Thoughts.
- LLM judges are noisy and may miss paraphrased leaks (question 11 in [[research-agenda]]).
- s1K is small, so effects may be small.
- Compute: evaluate at 32B, train at 7–8B.

## Related

- [[privacy-collapse]]
- [[privacy-leakage-in-agents]]
- [[prompt-defenses-for-contextual-privacy]]
