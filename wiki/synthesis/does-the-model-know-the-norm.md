---
type: synthesis
title: "Does the model know the norm? Probing, the awareness gap and steering repair"
tags: [contextual-integrity, interpretability, defenses]
area: [privacy-safety]
sources: [wang-2026-probing-steering-ci-norms, goel-2026-privacy-collapse, fischl-lanzoni-2026-breaking-bad]
created: 2026-11-23
updated: 2026-11-23
---

# Does the model know the norm?

From the discussion of [[wang-2026-probing-steering-ci-norms]] with Hamid (2026-11-23). The critiques below are Claude's reading of that paper, not findings of any source. Relevant to research question 12 (repair by steering) in [[research-agenda]].

## The claim

[[wang-2026-probing-steering-ci-norms]] reports a "privacy awareness gap": a linear readout separates appropriate from inappropriate flows (AUROC above 0.90 in deep layers), yet Llama-3.1 leaks in 42.5% of role-play scenarios. Summing one steering direction per [[contextual-integrity]] parameter then drives leakage to near zero.

## Why the gap may be smaller than it looks (Claude's critique)

- **Different prompts.** The norm is read when the model is explicitly asked "is this practice acceptable?"; leakage is measured in role-play. The probe is never read during generation, so the data show the model can *judge* the norm when asked, not that it *represents* it while acting.
- **Separability partly by construction.** The CI-parametric set varies one parameter at a time, and near-orthogonal directions are common in high dimensions. The evidence is representational, not causal.

## Why the steering result is not yet a repair (Claude's critique)

- **Utility collapse.** At α = 1.0, where CI-parametric leakage is 0.0–0.5% on Llama-3.1, Alpaca helpfulness, coherence and relevance are all 1.00 out of 5. Even at α = 0.5, helpfulness is 1.04. The same over-steering risk shows in [[fischl-lanzoni-2026-breaking-bad]] (seed), where strong steering made the model misreport failing tests.
- **The judge may reward broken output.** 90% of responses are judged "appropriate" at a dose where general utility is 1/5. The judge has no "incoherent" label and no human check.
- **Bigger push, not better axes?** Three unit vectors summed give a larger perturbation than one. Without a norm-matched or random-direction control, "three CI axes beat one" and "a bigger push beats a smaller one" cannot be told apart. (Breaking Bad reports a random-direction control that also helped in one case.)
- **Thin scale.** 200 and 270 evaluation items; main table, ablations and utility on Llama-3.1 only; Llama-2 gets worse on CONFAIDE under the method.

## A fair steering comparison would need

1. Leakage *and* utility at the same dose, for every method.
2. A norm-matched single direction and a random direction of the same norm.
3. A human-checked judge with an "incoherent" or "degenerate" label.
4. Probe readout during generation, on the same role-play prompts used for leakage.

## Contrast with privacy collapse

- [[goel-2026-privacy-collapse]]: after benign fine-tuning, the privacy steering direction of Llama-3-8B diverges in late layers and inverts in the final layer; behaviour and representation fail together.
- [[wang-2026-probing-steering-ci-norms]]: in base instruction-tuned models, the representation is intact (readable) while behaviour fails.
- Different conditions (fine-tuned vs base models, different directions and data), so these do not contradict each other directly. Both are recorded; a test of whether Wang et al.'s CI directions survive the fine-tuning in [[privacy-collapse]] would connect them.

## Related

- [[activation-probing-and-steering]]
- [[privacy-collapse]]
- [[contextual-integrity]]
- [[research-agenda]]
