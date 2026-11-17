---
type: source
title: "Leaky Thoughts: Large Reasoning Models Are Not Private Thinkers"
tags: [contextual-integrity, agents, defenses]
authors:
  - Tommaso Green
  - Martin Gubri
  - Haritz Puerto
  - Sangdoo Yun
  - Seong Joon Oh
venue: EMNLP
year: 2025
status: seed
url: https://arxiv.org/abs/2506.15674
created: 2026-11-17
updated: 2026-11-17
---

# Leaky Thoughts

> [!warning] Seed page
> Not yet ingested. The note below is a first pass; verify every number when ingesting, and add `zotero`.

arXiv 2506.15674; EMNLP 2025 main (ACL Anthology `2025.emnlp-main.1347`). Same group as [[goel-2026-privacy-collapse]] (Gubri, Yun, Oh).

## What they did

- **Question:** reasoning models used as personal agents are given the user's data. Do their reasoning traces leak it, even when the final answer does not?
- **Setup:** 13 open models (8B to 600B+): plain LLMs, chain-of-thought prompting and reasoning models (DeepSeek-R1 and distills, QwQ-32B, s1). Two benchmarks with a contextual-privacy framing: AirGapAgent-R (their open reconstruction of AirGapAgent, 4,160 probing questions) and AgentDAM (web agent tasks).
- **Findings (first pass):**
  - More test-time compute does not reliably make models more private.
  - A bigger reasoning budget makes the ==answer more careful but the reasoning leak more== (answer privacy about +10 points, reasoning privacy about −12).
  - Told to use placeholders in their reasoning, models almost never do; private data appears in roughly 19–78% of reasoning traces.
  - Reasoning leaks into the answer in about 6% of cases on average, and a prompt injection can extract the reasoning.
- **Mitigation:** RANA anonymises the reasoning before the answer is written. Small privacy gain, larger utility loss, and no effect on two models.

## How it relates to us (Claude's reading)

- **Another internal channel.** Same pattern as [[el-yagoubi-2026-agentleak|AgentLeak]]: the output looks clean while an internal channel leaks, and output-side checks miss it. The reasoning trace could be treated as one more channel next to inter-agent messages and memory. See [[privacy-leakage-in-agents]].
- **Reasoning as a defense cuts both ways.** [[lan-2025-ci-reasoning-rl]] and the AgentCIBench prompts ([[prompt-defenses-for-contextual-privacy]]) ask the model to reason about the flow before acting. This paper suggests that the reasoning itself may restate the private data, so a defense should be scored on the trace, not only on the answer.
- **Starting point for "why does leakage happen".** Most leaks in the reasoning are simple recollection of the data, while leaks in the answer are mostly attributed to misunderstanding the context. That split is a concrete handle for the understanding direction (question 7 in [[research-agenda]]).

## Where this fits

- [[privacy-leakage-in-agents]]
- [[prompt-defenses-for-contextual-privacy]]
- [[contextual-integrity]]
