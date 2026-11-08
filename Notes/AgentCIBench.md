
> [!Note]
> [[Goel 2026-09|Agents already leak out of the box]], and fine-tuning can make it worse, while standard benchmarks show neither.

>[!caution]
>contextual privacy should be a routine check before deploying any agent, and re-run after every model change.


1. **Privacy failures don’t need an attacker.** Much agent-security work focuses on adversaries, like prompt injection or jailbreaks. This paper shows a cooperative agent, doing exactly what you asked, still leaks by including too much. That’s a different threat model: **the risk comes from the agent’s own judgment, not from an outside attack.**

2. **Capable doesn’t mean careful.** Task completion and disclosure safety are separate axes. The most capable agents (Gemini-3.1-Pro, Gemini-3-Flash) were among the leakiest. Privacy has to be measured on its own.

3. **Privacy is about appropriate flows, not secrets.** Through [[Contextual Integrity|contextual integrity]]: the same item can be fine for one recipient and a leak for another. The agent always _sees_ everything, so the question is always whether this flow fits this context.

4. **Cheap fixes work surprisingly well.** Asking the agent to name the recipient and what’s appropriate for them, before writing, cut leakage by about 35 points _and_ also improved task success. <font color="#ffc000">Privacy and helpfulness aren’t always a trade-off.</font> Often the agent just isn’t thinking about the recipient.

----
