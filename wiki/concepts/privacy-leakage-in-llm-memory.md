---
type: concept
title: "Privacy leakage in LLM memory"
tags: [llm-memory, contextual-integrity, stub]
sources: [goel-2026-capable-but-careless, goel-2026-privacy-collapse]
created: 2026-11-09
updated: 2026-11-12
---

# Privacy leakage in LLM memory

> [!note]
> LLMs increasingly use persistent memory from past interactions to enhance personalization and task performance.
> Can models incorporate information from their memories appropriately?

This page is a stub. No dedicated memory source has been ingested yet.

## Evidence so far

- [[goel-2026-privacy-collapse]] uses CIMemories (100 sampled scenarios) as its memory setting. Models fine-tuned on "helpful" data include more inappropriate memory attributes, with a smaller drop than in the agentic setting (15% on average, as stated). For GPT-4.1, 74.2% of the CI failures it categorises are session-boundary failures. See [[privacy-collapse]].

## Sources to ingest

- CIMemories (mentioned in `my-notes/questions.md`): reportedly finds that prompting defenses do not reduce memory leakage. Cited by [[goel-2026-capable-but-careless]] as Mireshghallah et al., 2025 (arXiv 2511.14937), a persistent-memory CI benchmark.
- PersistBench (Pulipaka et al., ICML 2026): [[agentcibench|AgentCIBench]]'s scenario generator follows its MCTS method. Its memory relevance is not yet verified.

> [!note]
> [[goel-2026-capable-but-careless]] explicitly scopes long-term memory and multi-turn personalization out, so its finding that prompt defenses work does not directly speak to memory. See [[prompt-defenses-for-contextual-privacy]].

## Related

- [[contextual-integrity]]
- [[privacy-leakage-in-agents]]
- [[prompt-defenses-for-contextual-privacy]]
