---
type: entity
kind: benchmark
title: "PiSAs"
tags: [benchmark, agents, contextual-integrity, llm-memory]
sources: [gupta-2026-pisas]
created: 2026-11-14
updated: 2026-11-14
---

# PiSAs

A benchmark for [[contextual-integrity]] in multi-user agentic systems, where a team shares one system and information can spill between teammates. Introduced in [[gupta-2026-pisas]] (Gupta et al., arXiv 2026; facts below are from v2). Code at `github.com/ServiceNow/pisas`, data at `huggingface.co/datasets/ServiceNow/PiSAs`.

## At a glance

- **Unit:** a scenario = a team with roles, an executor who must produce a decision, peers holding information, and a set of attributes on a workplace timeline.
- **Labels:** each attribute has an appropriateness label (does the task need it?) and a visibility label (which users may see it?).
- **Pool:** 265 scenarios in nine task families across IT, healthcare, academia, aviation, cultural heritage and public administration; 5 to 10 agents per scenario.
- **Systems tested:** single agent, centralized (coordinator) and decentralized (token passing); memory none, private, shared or hybrid; privacy prompt None, Low, High.
- **Metrics:** appropriateness violations, visibility violations, failure rate, completeness, utility.
- **Scoring:** two-judge LLM ensemble (Hy3, GLM-5.3 Flash), 88.7% agreement with a human reference.

## Headline findings

- Mean appropriateness violations: 36.8% (single), 26.4% (centralized), 19.0% (decentralized); failure rate 79.6–100% in every configuration.
- Per-user agents cut violations but lose completeness.
- Memory raises completeness and moves violations onto memory surfaces.
- Attributes that look task-relevant leak; unrelated personal facts almost never do (0.2%).
- Output filters cut single-agent violations to 6% at a completeness cost; a small CI-trained model did worse than a generalist.

## Caveats

- Synthetic, cooperative setting; no adversary; one task per instance; memory lasts one run.
- Visibility labels come from LLM personas, with modest agreement with humans (κ = 0.37 over all recipients).
- v1 of the paper is a much smaller benchmark with different numbers; see the caution on [[gupta-2026-pisas]].

## Related

- [[cimemories]] (single user, persistent memory) and [[agentcibench]] (single user, computer use)
- [[privacy-leakage-in-agents]]
