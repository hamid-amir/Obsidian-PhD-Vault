---
type: source
title: "Capable but Careless: Do Computer-Use Agents Follow Contextual Integrity?"
tags: [agents, benchmark, defenses, contextual-integrity]
authors: [Anmol Goel]
venue: EMNLP
year: 2026
status: seed
created: 2026-11-09
updated: 2026-11-09
---

# Capable but Careless (AgentCIBench)

> [!warning] Seed page
> Written from Hamid's reading notes, not yet ingested from the full text. Verify the claims and numbers, and complete `authors`, `url` and `zotero`, when ingesting.

Introduces **AgentCIBench**, which measures whether computer-use agents (CUAs) make context-appropriate disclosure decisions under [[contextual-integrity]] constraints.

## The gap and the contribution

- The gap: we need to measure context-appropriate disclosure by agents. This is different from their performance (utility).
- AgentCIBench fills this gap by measuring the final disclosure decision of CUAs under contextual-integrity constraints.
- It is an ***evaluation harness***: a ==reusable== pipeline that can generate new tests as models improve.

## Key takeaways (Hamid's notes)

1. ==Privacy failures don't need an attacker.== Much agent-security work focuses on adversaries, like prompt injection or jailbreaks. This paper shows a cooperative agent, doing exactly what you asked, still leaks by including too much. That's a different threat model: **the risk comes from the agent's own judgment, not from an outside attack.**
2. **Capable doesn't mean careful.** Task completion and disclosure safety are separate axes. The most capable agents (Gemini-3.1-Pro, Gemini-3-Flash) were among the leakiest. Privacy has to be measured on its own.
3. **Privacy is about appropriate flows, not secrets.** Through [[contextual-integrity]]: the same item can be fine for one recipient and a leak for another. The agent always _sees_ everything, so the question is always whether this flow fits this context.
4. **Cheap fixes work surprisingly well.** Asking the agent to name the recipient and what's appropriate for them, before writing, cut leakage by about 35 points _and_ also improved task success. ==Privacy and helpfulness aren't always a trade-off.== Often the agent just isn't thinking about the recipient.

## Where this fits

- [[privacy-leakage-in-agents]]
- [[contextual-integrity]]
