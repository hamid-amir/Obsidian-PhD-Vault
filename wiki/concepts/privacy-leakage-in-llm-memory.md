---
type: concept
title: "Privacy leakage in LLM memory"
tags: [llm-memory, contextual-integrity]
sources: [mireshghallah-2025-cimemories, goel-2026-capable-but-careless, goel-2026-privacy-collapse]
created: 2026-11-09
updated: 2026-11-12
---

# Privacy leakage in LLM memory

> [!note]
> LLMs increasingly use persistent memory from past interactions to enhance personalization and task performance.
> Can models incorporate information from their memories appropriately?

## Evidence so far

- [[mireshghallah-2025-cimemories]] ([[cimemories|CIMemories]]): given a user's stored memories and a task with a recipient, frontier models reveal attributes they should withhold. Violation@5 ranges from 14.82% (GPT-4o) to 69.14% (Qwen-3 32B), with completeness around 50% for every model.
  - Violations grow with repeated sampling (GPT-5: 9.6% at one sample, 25.1% at five) and with more not-to-share memories, while completeness stays flat.
  - Models often pick the right information domain but over-share within it ("granularity failure").
  - Reasoning reduced violations in the one model family tested; model size saturated; minimisation-style prompts traded completeness for privacy (see [[prompt-defenses-for-contextual-privacy]]).
  - Caveats: labels are GPT-5 consensus without reported human validation, the metric is a worst case over tasks and samples, single turn, no tools.
- [[goel-2026-privacy-collapse]] uses [[cimemories|CIMemories]] (100 sampled scenarios, different judge) as its memory setting. Models fine-tuned on "helpful" data include more inappropriate memory attributes, with a smaller drop than in the agentic setting (15% on average, as stated). For GPT-4.1, 74.2% of the CI failures it categorises are session-boundary failures. See [[privacy-collapse]].

## Sources to ingest

- PersistBench (Pulipaka et al., ICML 2026): [[agentcibench|AgentCIBench]]'s scenario generator follows its MCTS method. Its memory relevance is not yet verified.

> [!note]
> [[goel-2026-capable-but-careless]] explicitly scopes long-term memory and multi-turn personalization out, so its finding that prompt defenses work does not directly speak to memory. Conversely, CIMemories tested only minimisation-style prompts, so it does not show that recipient-aware prompting fails for memory. See [[prompt-defenses-for-contextual-privacy]].

## Open questions

- Would recipient-aware prompts break the violation–completeness trade-off on CIMemories? Untested in either paper.
- How much of the measured "accumulation" is behaviour and how much is the worst-case metric?

## Related

- [[cimemories]]
- [[contextual-integrity]]
- [[privacy-leakage-in-agents]]
- [[prompt-defenses-for-contextual-privacy]]
