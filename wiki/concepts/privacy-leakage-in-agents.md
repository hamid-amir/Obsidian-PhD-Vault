---
type: concept
title: "Privacy leakage in agents"
tags: [agents, contextual-integrity]
sources: [goel-2026-capable-but-careless, goel-2026-privacy-collapse]
created: 2026-11-09
updated: 2026-11-09
---

# Privacy leakage in agents

> [!Note]
> [[goel-2026-capable-but-careless|Agents already leak out of the box]], and [[goel-2026-privacy-collapse|fine-tuning can make it worse]], while standard benchmarks show neither.

> [!caution]
> Contextual privacy should be a routine check before deploying any agent, and re-run after every model change.

Leakage here is judged through [[contextual-integrity]]: the agent sees everything, so the question is whether a given flow fits its context, not whether the information is secret.

## Evidence so far

- [[goel-2026-capable-but-careless]] (AgentCIBench): cooperative computer-use agents leak without any attacker; capability and disclosure safety are separate axes; a cheap recipient-aware prompt reduced leakage and improved task success.
- [[goel-2026-privacy-collapse]]: benign fine-tuning can break contextual privacy. Not yet ingested from the full text.

## Open questions

- Why do prompting defenses help here but reportedly not for memory leakage? See `my-notes/questions.md` and [[privacy-leakage-in-llm-memory]].
