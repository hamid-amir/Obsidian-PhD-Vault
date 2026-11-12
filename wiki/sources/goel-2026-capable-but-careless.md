---
type: source
title: "Capable but Careless: Do Computer-Use Agents Follow Contextual Integrity?"
tags:
  - agents
  - benchmark
  - defenses
  - contextual-integrity
authors:
  - Anmol Goel
  - Iryna Gurevych
venue: EMNLP
year: 2026
status: ingested
url: https://arxiv.org/abs/2606.23189
ingested: 2026-11-09
created: 2026-11-09
updated: 2026-11-12
---

# Capable but Careless (AgentCIBench)

> [!info] Source
> Goel & Gurevych, UKP Lab (TU Darmstadt) and ATHENE. arXiv 2606.23189 (v1 22 Jun 2026, v2 10 Sep 2026), comments line: "EMNLP 2026 Main". Code: `github.com/UKPLab/emnlp2026-agentcibench`; data: `hf.co/datasets/UKPLab/AgentCIBench`. Ingested from the arXiv HTML full text (end of Appendix P, Tables 15–16 and the Appendix Q prompt figures were not read).

Introduces **[[agentcibench|AgentCIBench]]**, which measures whether computer-use agents (CUAs) make context-appropriate disclosure decisions under [[contextual-integrity]] constraints.

![[Pasted image 20261109194033.png]]
## The gap and the contribution

- The gap: we need to measure context-appropriate disclosure by agents. This is different from their performance (utility).
- AgentCIBench fills this gap by measuring the final disclosure decision of CUAs under contextual-integrity constraints.
- It is an ***evaluation harness***: a ==reusable== pipeline that can generate new tests as models improve.
- The paper positions itself against capability benchmarks (task completion), security benchmarks (adversarial robustness, e.g. AgentDojo, InjecAgent) and prior CI evaluations for LLMs (ConfAIde, PrivacyLens, CI-Bench), which it describes as text-only, single-app and single-turn. Its claimed gap has three parts: multi-app computer use, multi-turn execution with persistent UI state, and normative (CI-based) scoring.
- Threat model: ==unintentional leakage by a cooperative agent, with no adversary==.

## Setup

- A scenario is (app state, task prompt, recipient, must-share items `V_share`, must-not-share items `V_leak`). The agent produces one externally visible action (sent message, saved calendar event, posted note or reply).
- **Utility** = every must-share item is in the action. **Leakage** = any must-not-share item is in the action. **Refusal** = empty or deflecting action.
- Primary metric: **engagement-conditioned leakage** `L_eng = L / (1 − R)`, so that an agent cannot look private by refusing.
- Three failure modes, each a perturbation of one CI parameter (see [[privacy-leakage-in-agents#Failure modes]]):
  - **Visual co-location (VCL):** sensitive items sit next to task-relevant ones in the UI.
  - **Task-ambiguity overshare (TAO):** an under-specified request ("draft a status update") invites including too much.
  - **Recipient misalignment (RMA):** content suited to one recipient is sent to another.
- The paper estimates 30.2% of scenarios also instantiate a secondary mode, so per-mode rates are lower bounds.

## How the benchmark is built

- Scenarios are surfaced by Monte Carlo Tree Search over ~36 everyday seed requests, with one mutation strategy per failure mode. The method follows PersistBench (Pulipaka et al., ICML 2026).
- Search reward = utility × judge CI-violation severity; severity only guides search, headline results use binary leakage.
- Generation models: mutator DeepSeek-v4-Pro, judge Gemma-4-31B-IT, proxy agents Qwen-3.6-35B-A3B, Kimi-K2.5, MiniMax-M2.5.
- Pipeline yield: 480 candidates → 140 retained → 117 final scenarios from 28 seeds (75 TAO, 18 VCL, 24 RMA). Followed by manual checks on sampled scenarios.
- Environment: OpenApps (Ullrich et al., ICLR 2026) on BrowserGym, six apps (Messenger, Calendar, Maps, ToDo, Code Editor, Shop).
- Scoring is hybrid: a deterministic string matcher plus an LLM judge. Reported reliability: matcher and judge agree on 97.7% of leak outcomes; human annotators reach Cohen's κ = 0.89 against the scorer; swapping in GPT-5.4-mini as judge reproduces 99.8% of leak verdicts with identical model ranking.

## Results

Main study: 15 agents (via OpenRouter, temperature 0.2), all 117 scenarios, state-grounded (app state given as structured text, agent emits one action JSON).

- Average utility 68.8%, average leakage 67.9%. 12 of 15 agents leak on more than half of the scenarios (but see the caveat below).
- No scenario is leaked by zero agents.
- ==Utility and L_eng are only weakly correlated== (Pearson r = 0.49, p = 0.06). Among agents with utility above 75%, L_eng ranges from 14.0 (Claude-Opus-4.7) to 98.3 (Gemini-3.1-Pro).
- **Refusal masks leakage:** GPT-5.4 has raw leakage 18.8 partly by refusing 41.9% of scenarios; its L_eng is 32.4.

Selected rows of Table 1 (%):

| Agent | Utility | Leakage | Refusal | L_eng |
|---|---|---|---|---|
| Claude-Opus-4.7 | 81.2 | 13.7 | 1.7 | 14.0 |
| GPT-5.4 | 44.4 | 18.8 | 41.9 | 32.4 |
| Claude-Sonnet-4.6 | 52.1 | 46.2 | 15.4 | 54.5 |
| DeepSeek-v4-Pro | 64.1 | 82.9 | 2.6 | 85.1 |
| Gemini-3-Flash | 88.0 | 93.2 | 0.9 | 94.0 |
| Qwen-3.6-Max | 82.1 | 97.4 | 0.0 | 97.4 |
| Gemini-3.1-Pro | 96.6 | 98.3 | 0.0 | 98.3 |

Per failure mode (Table 2), the paper reads three patterns: safety-tuned models keep TAO and RMA low but not VCL (Opus: VCL 33.3 vs TAO 9.3); Kimi-K2.6 suppresses VCL and RMA largely by refusing but spikes on TAO (81.3); the leakiest models saturate every mode.

**End-to-end in the UI** (Opus and Sonnet, 50-scenario subset, screenshot + accessibility tree, 20-step budget): engaged runs leak more than in the state-grounded setting (Opus 42.9% vs 14.0%; Sonnet 80.0% vs 54.5%). Most runs did not complete within the budget, and only 14 (Opus) and 10 (Sonnet) runs were engaged, so these estimates are wide. Access mode (accessibility tree only vs mixed) showed no detectable difference.

A transfer check on two later models (GPT-5.6-terra, GPT-5.6-luna, released after generation) gives L_eng 28.1% and 45.0%.

## Defenses

Three prompt-only defenses prepended to the system message, tested on Claude-Opus-4.7, GPT-5.4 and DeepSeek-v4-Pro (Table 11, averaged, %). See [[prompt-defenses-for-contextual-privacy]].

| Defense | Utility | L_eng | ΔU | ΔL_eng |
|---|---|---|---|---|
| None | 63.2 | 51.7 | – | – |
| Restrictive (read only needed fields) | 78.9 | 19.0 | +15.7 | −32.7 |
| Rubric-informed (4-point CI rubric) | 79.2 | 15.8 | +16.0 | −35.9 |
| Recipient-typed (name recipient and norms first) | 86.3 | 16.2 | +23.1 | −35.5 |

- Largest per-model effect: DeepSeek-v4-Pro L_eng 92.4 → 29.1 under Recipient-typed.
- Under Recipient-typed, all three modes drop (VCL 70.6 → 19.6, TAO 45.9 → 15.0, RMA 60.9 → 17.2).
- A pairwise output-quality judge (privacy excluded) found Recipient-typed and Rubric-informed won or tied in more than 60% of comparisons.
- **External filter baseline:** a separate model redacting drafts without task context cut leakage (37.7% → 10.2%) but collapsed utility (63.3% → 22.5%). These numbers appear in the text, not in a table.
- Stated limits: three models and three prompts; no fine-tuning or other non-prompt interventions.

## Limitations (as stated)

- Synthetic, adversarially generated pool: absolute rates are a stress test and should be read as relative orderings, not real-world harm estimates.
- OpenApps is a controlled six-app workspace. The end-to-end study covers two agents on 50 scenarios.
- The pool is skewed toward TAO, and the modes overlap.
- ==Long-term memory, multi-turn personalization, and professional or coding-agent contexts are out of scope.==
- CI labels are scenario-specific and may vary by culture or organization.

The paper's policy appendix argues that CI testing should be a pre-deployment check and be re-run after later training stages, citing [[goel-2026-privacy-collapse]] for the claim that benign fine-tuning can erode contextual-privacy behavior.

> [!caution] Internal inconsistencies in the paper
> Found while reading; recorded rather than resolved.
> - **Leak count:** the v2 abstract on the arXiv abstract page says 11 of 15 agents leak on more than half the scenarios. The HTML body and Table 1 give 12 of 15.
> - **"Two leakiest":** the text names Gemini-3.1-Pro and Gemini-3-Flash, but Table 1 puts Qwen-3.6-Max (97.4) above Gemini-3-Flash (94.0).
> - **End-to-end choice:** the text says Opus and Sonnet have the two lowest L_eng, but GPT-5.4 (32.4) is below Sonnet (54.5).
> - **Defense range:** §8 text cites prompt-defense leakage of 12.5–15.4%; Table 11 gives L_eng 15.8–19.0.
> - **Judge audit:** §5 describes a 10% subset for the κ = 0.89 audit; Appendix F describes 50 runs covering 36% of the pool.
> - **End-to-end counts:** Appendix N says 99 of 100 runs were truncated, which is hard to reconcile with Table 14's non-completion rates (64%, 72%).
> - **Generation-family overlap:** Appendix C says 12 of 15 evaluated agents come from families unused in generation, but DeepSeek-v4-Pro (mutator) and Qwen-3.6-35B-A3B (proxy) are themselves evaluated, and other families overlap.

## Key takeaways (Hamid's notes)

1. ==Privacy failures don't need an attacker.== Much agent-security work focuses on adversaries, like prompt injection or jailbreaks. This paper shows a cooperative agent, doing exactly what you asked, still leaks by including too much. That's a different threat model: **the risk comes from the agent's own judgment, not from an outside attack.**
2. **Capable doesn't mean careful.** Task completion and disclosure safety are separate axes. The most capable agents (Gemini-3.1-Pro, Gemini-3-Flash) were among the leakiest. Privacy has to be measured on its own.
3. **Privacy is about appropriate flows, not secrets.** Through [[contextual-integrity]]: the same item can be fine for one recipient and a leak for another. The agent always _sees_ everything, so the question is always whether this flow fits this context.
4. **Cheap fixes work surprisingly well.** Asking the agent to name the recipient and what's appropriate for them, before writing, cut leakage by about 35 points _and_ also improved task success. ==Privacy and helpfulness aren't always a trade-off.== Often the agent just isn't thinking about the recipient.

> [!note] Checked against the full text
> All four takeaways hold. On (2): Gemini-3.1-Pro and Gemini-3-Flash have the two highest utilities (96.6, 88.0) and L_eng of 98.3 and 94.0; Qwen-3.6-Max sits between them. On (4): the ~35-point figure is the Recipient-typed average over three models (−35.5 L_eng, +23.1 utility); the per-model effect ranges from −8.0 (Opus) to −63.3 (DeepSeek).

## Where this fits

- [[agentcibench]]
- [[privacy-leakage-in-agents]]
- [[prompt-defenses-for-contextual-privacy]]
- [[contextual-integrity]]
- Cites [[mireshghallah-2025-cimemories|CIMemories]] (Mireshghallah et al., 2025) as a persistent-memory CI benchmark but scopes memory out: see [[privacy-leakage-in-llm-memory]].
