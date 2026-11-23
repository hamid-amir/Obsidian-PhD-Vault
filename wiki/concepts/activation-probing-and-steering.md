---
type: concept
title: "Activation probing and steering"
tags: [interpretability]
sources: [goel-2026-privacy-collapse, wang-2026-probing-steering-ci-norms, rose-2026-multi-agent-collusion-detection, westphal-2026-hide-and-seek-embedding, fischl-lanzoni-2026-breaking-bad]
created: 2026-11-21
updated: 2026-11-23
---

# Activation probing and steering

> [!Definition]
> Treating a behaviour or property as a **direction in a model's internal activations**, then either **reading** it (probing, monitoring) or **writing** it (steering: adding or subtracting the direction during generation).

[[goel-2026-privacy-collapse]] and [[wang-2026-probing-steering-ci-norms]] are ingested; the other three sources are seeds, so their details below are unverified.

## Building a direction

- **Difference of means on contrasting data.** Privacy and commonsense steering vectors in [[goel-2026-privacy-collapse]] (§5.2); a "task badness" direction from stories of tasks going well vs badly in [[fischl-lanzoni-2026-breaking-bad]] (seed).
- **One direction per CI parameter.** Separate directions for information type, recipient and transmission principle, each from 500 synthetic examples that vary only that parameter, unit-normalised and summed for steering ([[wang-2026-probing-steering-ci-norms]]).
- **PCA on paired differences.** First principal component of matched appropriate/inappropriate pairs, as a reading direction for CI norms ([[wang-2026-probing-steering-ci-norms]]).
- **Trained linear probes.** Per-agent deception probes in [[rose-2026-multi-agent-collusion-detection]] (seed); probes on later-layer hidden states in [[westphal-2026-hide-and-seek-embedding]] (seed).

## Reading it

- **Detecting hidden behaviour the output does not show:** collusion between agents ([[rose-2026-multi-agent-collusion-detection]], seed) and secrets encoded in fluent text ([[westphal-2026-hide-and-seek-embedding]], seed).
- **Is the norm there at all?** In four base instruction-tuned models, whether a flow is appropriate is linearly readable (AUROC above 0.90 in deep layers; synthetic-trained probe transfers to CONFAIDE at up to 0.84), while the same models still leak in role-play: the "privacy awareness gap" ([[wang-2026-probing-steering-ci-norms]]). The norm is read under an explicit "is this acceptable?" prompt, not during generation; see [[does-the-model-know-the-norm]].
- **Diagnosing training damage:** after fine-tuning, privacy vectors diverge from the base model's in late layers and invert in the final layer, while commonsense vectors stay aligned ([[goel-2026-privacy-collapse]]).
- **Scoring training data:** filtering samples by their projection on the privacy direction partly prevents [[privacy-collapse]] ([[goel-2026-privacy-collapse]]).
- **Early warning in agent runs:** the task-badness signal rises over the turns before a reward hack, but is a weak per-run predictor: most non-cheating runs reach the same level ([[fischl-lanzoni-2026-breaking-bad]], seed).

## Writing it (steering)

- Summed per-CI-parameter steering drives leakage on Llama-3.1 from 42.5% to 0.5% on synthetic scenarios and to 0.0% on CONFAIDE and PrivaCI-Bench, beating single-direction steering, LoRRA and representation tuning. Single axes alone raise CONFAIDE leakage. General utility at that dose is 1/5 ([[wang-2026-probing-steering-ci-norms]]).

- Steering against task badness cuts reward hacking in one model; adding it raises hacking. Effects are weaker or absent in two other models, and a random-direction control also helped in one case ([[fischl-lanzoni-2026-breaking-bad]], seed).

## Cautions for privacy work

- **A direction may encode more than its label.** In [[fischl-lanzoni-2026-breaking-bad]] (seed), the mood part of the "task badness" direction predicts and steers better than the task-state part. A "privacy" direction could likewise carry caution or negativity.
- **Strong steering can break perception, not just behaviour.** At the largest dose in [[fischl-lanzoni-2026-breaking-bad]] (seed), the model reported failing tests as passing. Any repair by steering (research question 12 in [[research-agenda]]) should check that the model still registers sensitive content, not only that it discloses less.
- **Low leakage can mean a broken model.** In [[wang-2026-probing-steering-ci-norms]], the dose that gives 0% leakage scores 1.00/5 on helpfulness, coherence and relevance for general instructions, while an LLM judge still rates 90% of privacy responses "appropriate". Report utility at the same dose and compare against norm-matched and random directions ([[does-the-model-know-the-norm]]).

> [!note] Claude's suggestion (2026-11-21, untested)
> If a "going badly" state pushes agents to break rules to finish a task, it may also push them to disclose information they should not. Reading this direction on agent privacy benchmarks ([[agentcibench]], [[pisas]]) would test that. Nothing in the wiki measures it.

## Related

- [[privacy-collapse]]
- [[does-the-model-know-the-norm]] (synthesis: awareness gap and a fair steering comparison)
- [[privacy-leakage-in-agents]] (includes notes on a word-level readout used for monitoring)
- [[research-agenda]]
