---
type: entity
kind: benchmark
title: "CIMemories"
tags: [benchmark, llm-memory, contextual-integrity]
sources: [mireshghallah-2025-cimemories, goel-2026-privacy-collapse, goel-2026-capable-but-careless]
created: 2026-11-12
updated: 2026-11-12
---

# CIMemories

A benchmark for whether a memory-augmented LLM assistant shares stored user memories appropriately for each task and recipient, introduced in [[mireshghallah-2025-cimemories]] (Mireshghallah, Mangaokar et al., FAIR at Meta, arXiv 2025).

![[Pasted image 20261112174617.png]]
## At a glance

- **Unit:** a synthetic user profile (about 147 memory attributes) plus a task with a recipient. The model sees the memory block and writes one message.
- **Labels:** each attribute–task pair is labelled share or withhold by GPT-5 under three Westin privacy personas; only unanimous pairs are scored.
- **Metrics:** Violation@n (attribute-level, worst case over tasks and n samples) and completeness (average share of to-share attributes revealed).
- **Pool:** 10 profiles, about 46 tasks per profile, about 7 to-share and 84 not-to-share attributes per task.
- **Scoring:** DeepSeek-R1 judge, explicit reveals only.

## Headline findings

- Violation@5 ranges from 14.82% (GPT-4o) to 69.14% (Qwen-3 32B); completeness is around 50% for all models.
- Violations grow with more samples and more stored memories.
- Minimisation-style prompts trade completeness for fewer violations (GPT-5 only). See [[prompt-defenses-for-contextual-privacy]].

## Used by

- [[goel-2026-privacy-collapse]] uses 100 sampled scenarios with a gpt-5-nano judge as its memory setting, so its numbers are not comparable to the original Table 1.
- [[goel-2026-capable-but-careless]] cites it as a persistent-memory CI benchmark and scopes memory out of [[agentcibench|AgentCIBench]].

## Caveats

- Labels are model consensus (GPT-5), with no human validation reported, and GPT-5 is also evaluated.
- Single turn, no tools, synthetic profiles.
- See the caution callout on [[mireshghallah-2025-cimemories]] for inconsistencies in the paper.
