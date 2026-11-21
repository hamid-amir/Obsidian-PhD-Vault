---
type: concept
title: "Activation probing and steering"
tags: [interpretability]
sources: [goel-2026-privacy-collapse, wang-2026-probing-steering-ci-norms, rose-2026-multi-agent-collusion-detection, westphal-2026-hide-and-seek-embedding, fischl-lanzoni-2026-breaking-bad]
created: 2026-11-21
updated: 2026-11-21
---

# Activation probing and steering

> [!Definition]
> Treating a behaviour or property as a **direction in a model's internal activations**, then either **reading** it (probing, monitoring) or **writing** it (steering: adding or subtracting the direction during generation).

Only [[goel-2026-privacy-collapse]] is ingested; the other three sources are seeds, so their details below are unverified.

## Building a direction

- **Difference of means on contrasting data.** Privacy and commonsense steering vectors in [[goel-2026-privacy-collapse]] (§5.2); a "task badness" direction from stories of tasks going well vs badly in [[fischl-lanzoni-2026-breaking-bad]] (seed).
- **One direction per CI parameter.** Separate directions for information type, recipient and transmission principle, summed for steering, in [[wang-2026-probing-steering-ci-norms]] (seed).
- **Trained linear probes.** Per-agent deception probes in [[rose-2026-multi-agent-collusion-detection]] (seed); probes on later-layer hidden states in [[westphal-2026-hide-and-seek-embedding]] (seed).

## Reading it

- **Detecting hidden behaviour the output does not show:** collusion between agents ([[rose-2026-multi-agent-collusion-detection]], seed) and secrets encoded in fluent text ([[westphal-2026-hide-and-seek-embedding]], seed).
- **Diagnosing training damage:** after fine-tuning, privacy vectors diverge from the base model's in late layers and invert in the final layer, while commonsense vectors stay aligned ([[goel-2026-privacy-collapse]]).
- **Scoring training data:** filtering samples by their projection on the privacy direction partly prevents [[privacy-collapse]] ([[goel-2026-privacy-collapse]]).
- **Early warning in agent runs:** the task-badness signal rises over the turns before a reward hack, but is a weak per-run predictor: most non-cheating runs reach the same level ([[fischl-lanzoni-2026-breaking-bad]], seed).

## Writing it (steering)

- Steering against task badness cuts reward hacking in one model; adding it raises hacking. Effects are weaker or absent in two other models, and a random-direction control also helped in one case ([[fischl-lanzoni-2026-breaking-bad]], seed).

## Cautions for privacy work

- **A direction may encode more than its label.** In [[fischl-lanzoni-2026-breaking-bad]] (seed), the mood part of the "task badness" direction predicts and steers better than the task-state part. A "privacy" direction could likewise carry caution or negativity.
- **Strong steering can break perception, not just behaviour.** At the largest dose in [[fischl-lanzoni-2026-breaking-bad]] (seed), the model reported failing tests as passing. Any repair by steering (research question 12 in [[research-agenda]]) should check that the model still registers sensitive content, not only that it discloses less.

> [!note] Claude's suggestion (2026-11-21, untested)
> If a "going badly" state pushes agents to break rules to finish a task, it may also push them to disclose information they should not. Reading this direction on agent privacy benchmarks ([[agentcibench]], [[pisas]]) would test that. Nothing in the wiki measures it.

## Related

- [[privacy-collapse]]
- [[privacy-leakage-in-agents]] (includes notes on a word-level readout used for monitoring)
- [[research-agenda]]
