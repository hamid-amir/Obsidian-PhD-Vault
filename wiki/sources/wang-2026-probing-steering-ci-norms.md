---
type: source
title: "Do LLMs Know What Is Private Internally? Probing and Steering Contextual Privacy Norms in Large Language Model Representations"
tags: [contextual-integrity, interpretability, defenses]
authors:
  - Haoran Wang
  - Li Xiong
  - Kai Shu
venue: arXiv
year: 2026
status: ingested
url: https://arxiv.org/abs/2604.00209
citations: 4
citations_updated: 2026-11-24
ingested: 2026-11-23
created: 2026-11-21
updated: 2026-11-23
---

# Do LLMs Know What Is Private Internally?

> [!info] Source
> Haoran Wang, Li Xiong, Kai Shu. arXiv 2604.00209 (cs.CL), 2026. No venue listed.

Probes open models for [[contextual-integrity]] norms. Claims three things: models encode whether a flow is appropriate yet still leak in role-play (a "privacy awareness gap"); three CI parameters sit in separable directions; and steering with one direction per parameter, summed, cuts leakage far more than single-direction steering. See [[does-the-model-know-the-norm]] for a critical reading.

## Setup

- **Models:** Llama-3.1-8B-Instruct, Qwen-2.5-7B-Instruct, Mistral-7B-Instruct-v0.3, Llama-2-7B-Chat. Last-token hidden state at every layer.
- **Three synthetic probe sets, built from CI templates:**
  - **Concept:** 500 matched pairs over ten information types that differ in recipient or transmission principle, framed as "Evaluate whether the following information practice is acceptable or unacceptable from a privacy perspective".
  - **Behavioural:** 200 role-play scenarios (100 inappropriate, 100 appropriate) from secret-keeping vs legitimate-sharing templates.
  - **CI-parametric:** 100 base contexts, each varied over five values of one parameter (information type, recipient, transmission principle): 1,500 examples.
- **Reading methods:** PCA on mean-centred paired differences (first PC, signed so positive = appropriate) and per-layer logistic probes (5-fold CV).
- **External tests:** CONFAIDE Tier 2 (probe transfer) and Tier 3 (270 items, leakage), PrivaCI-Bench (leakage).

## Findings

- **Norms are readable.** PCA AUROC rises from 0.53 in early layers to above 0.90 in deep layers on synthetic data. On CONFAIDE Tier 2, PCA gives 0.43 with one PC and 0.77 with three (no gain from more); a probe trained on synthetic data transfers at up to 0.84.
- **"Privacy awareness gap."** Llama-3.1 still leaks in 42.5% of the 200 behavioural scenarios. Unsteered CONFAIDE Tier 3 leakage: Llama-3.1 24.1%, Qwen-2.5 38.5%, Mistral 25.9%, Llama-2 23.7%.
- **Separable CI parameters.** An LDA subspace per parameter classifies its own parameter near-perfectly; projecting the other parameters onto it gives 19–27% (chance 20%). Cosine similarities between parameter directions fall below a permutation null by 6.1σ (Llama-3.1, Qwen), 3.7σ (Llama-2) and 2.1σ (Mistral, p = 0.05).

## CI-parametric steering

- **Method:** h′ = h + α Σ v(p), one unit-normalised direction per CI parameter, applied at the top-5 layers. Baselines: single-direction additive steering, LoRRA, representation tuning.
- **Judging:** GPT-4o-mini (temperature 0) labels each response leaked, refused or appropriate. Metrics: leakage rate, NCR (share judged appropriate) and PPI (relative leakage reduction vs unsteered).
- **Synthetic, Llama-3.1, α = 1.0 (leakage %):** none 42.5; additive 21.5; LoRRA 16.0 (NCR 84.0); representation tuning 37.0; CI-parametric 0.5 (NCR 90.0, PPI 98.8).
- **Transfer, CI-parametric α = 1.0 (leakage %, Llama-3.1 / Qwen / Mistral / Llama-2):**
  - CONFAIDE Tier 3: 0.0 / 15.2 / 1.9 / 43.3 (unsteered 24.1 / 38.5 / 25.9 / 23.7).
  - PrivaCI-Bench: 0.0 / 12.0 / 0.7 / 19.3 (unsteered 18.0 / 16.5 / 10.7 / 22.5).
  - Single-direction additive steering often raises leakage (PrivaCI, Llama-3.1: 64.7). Llama-2 gets worse on CONFAIDE under CI-parametric steering.
- **Ablation (Llama-3.1):** each single axis helps on synthetic data (recipient 18.5, information type 23.0, transmission principle 27.5), but on CONFAIDE (α = 0.5) each raises leakage above the 24.1 baseline (58.5 / 62.6 / 53.3); all three together give 2.6. Adding subject and sender as fourth and fifth axes does worse on synthetic data (16.0).
- **Utility (Appendix H, Llama-3.1, 200 Alpaca prompts, GPT-4o-mini scores 1–5):**

| Steering | Helpfulness | Coherence | Relevance |
|---|---|---|---|
| None | 4.58 | 4.76 | 4.89 |
| Monolithic (single direction) α = 0.5 | 4.13 | 4.61 | 4.62 |
| CI-parametric α = 0.5 | 1.04 | 2.53 | 1.51 |
| Monolithic (single direction) α = 1.0 | 1.07 | 2.40 | 1.28 |
| CI-parametric α = 1.0 | 1.00 | 1.00 | 1.00 |

The authors call the utility cost "manageable".

## Gaps and limitations

- No limitations section; the conclusion names adaptive or evolving attacks as future work.
- The headline leakage numbers come at doses where general-instruction utility is at or near the floor (table above).
- No human check of the judge, no example steered outputs, no "incoherent" label.
- No norm-matched or random-direction control for the summed three-direction vector.
- Main steering table, ablations and utility are on Llama-3.1 only; evaluation sets are 200 and 270 items.
- The 200 synthetic evaluation scenarios are part of the probing stimuli; whether the baselines' directions were extracted from them is not stated.

## Where this fits

- [[activation-probing-and-steering]]
- [[contextual-integrity]]
- [[privacy-collapse]] (contrast with [[goel-2026-privacy-collapse]])
- [[does-the-model-know-the-norm]]
