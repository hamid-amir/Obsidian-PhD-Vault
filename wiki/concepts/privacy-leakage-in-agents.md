---
type: concept
title: "Privacy leakage in agents"
tags: [agents, contextual-integrity, defenses]
sources: [goel-2026-capable-but-careless, goel-2026-privacy-collapse, gupta-2026-pisas]
created: 2026-11-09
updated: 2026-11-15
---

# Privacy leakage in agents

> [!Note]
> [[goel-2026-capable-but-careless|Agents already leak out of the box]], and [[goel-2026-privacy-collapse|fine-tuning can make it worse]], while standard benchmarks show neither.

> [!caution]
> Contextual privacy should be a routine check before deploying any agent, and re-run after every model change.

Leakage here is judged through [[contextual-integrity]]: the agent sees everything, so the question is whether a given flow fits its context, not whether the information is secret.

## Evidence so far

- [[goel-2026-capable-but-careless]] ([[agentcibench|AgentCIBench]]): cooperative computer-use agents leak without any attacker. Across 15 agents, mean leakage is 67.9%, and utility and engagement-conditioned leakage are only weakly correlated (r = 0.49), so capability and disclosure safety are separate axes. Leakage was higher, not lower, when two agents acted end-to-end in the UI (small sample). Cheap prompt defenses reduced leakage and improved task success: see [[prompt-defenses-for-contextual-privacy]].
- Measuring leakage needs care with refusals: an agent that refuses often looks private on raw leakage. AgentCIBench therefore conditions on engagement (`L_eng = L / (1 − R)`) ([[goel-2026-capable-but-careless]]).
- [[goel-2026-privacy-collapse]] ([[privacy-collapse]]): benign fine-tuning can break contextual privacy. Fine-tuning on responses that use accessible context without asking cut PrivacyLens accuracy sharply (gpt-4o-mini 84.18 → 1.56), and empathetic or customer-support dialogue data cut it by roughly 17–24% relative, while safety and capability benchmarks barely moved. The effect can also be hidden behind a trigger string (§4.5).
- [[gupta-2026-pisas]] ([[pisas|PiSAs]]): when a team shares one agentic system, information spills between teammates. With one agent serving everyone, mean appropriateness violations are 36.8%; giving each user an agent lowers this to 26.4% (centralized) or 19.0% (decentralized) but loses completeness, and 79.6–100% of runs still fail. What leaks is mostly task-adjacent: private details attached to needed facts (35.7%) and distractors (42.9%), against 0.2% for unrelated personal facts.

## Settings: who shares the system

| Setting | Users | Agents | Example source |
|---|---|---|---|
| Single user | One | One | [[cimemories]], [[agentcibench]] |
| Multi-user, shared system | A team | One, or one per user | [[pisas]] |
| Independently owned agents | Many | One per user, each owned by its user | MAGPIE (not ingested) |
| One user, many agents | One | Several | AgentLeak (not ingested) |

The first three rows follow [[gupta-2026-pisas]]; the last row is not yet backed by an ingested source.

## Failure modes

[[goel-2026-capable-but-careless]] names three, each perturbing one [[contextual-integrity]] parameter:

- **Visual co-location:** sensitive items sit next to task-relevant ones, so the agent pulls them in (perturbs which attributes are surfaced).
- **Task-ambiguity overshare:** an under-specified request invites including too much (perturbs the transmission principle).
- **Recipient misalignment:** content suited to one recipient goes to another (perturbs the recipient).

Safety-tuned models in that study kept the last two low but still leaked under visual co-location.

[[gupta-2026-pisas]] adds a multi-user distinction: an **appropriateness violation** (using something the task does not need) versus a **visibility violation** (information reaching a teammate who should not see it). The second can occur inside the system, in messages, agent context or memory, without appearing in any output.

> [!note] Side note: J-lens as a possible analysis tool
> From a blog post, [J-Space: Yet Another LLM Mind Reader?](https://huggingface.co/blog/dlouapre/j-space) (David Louapre, Hugging Face, 13 Jul 2026), which explains Anthropic's J-lens method. Read on 2026-11-15 from the page text; figures and the underlying Anthropic paper were not read. The post says nothing about privacy.
> - **What it is:** an improved logit lens that reads which words an internal activation is "poised to make the model say", including unspoken intermediate steps (==basically J-spcae is the model's whiteboard==). Swapping one concept for another can change the answer. One fit per model on unlabelled text; pre-fitted lenses for open models are on Neuronpedia.
> - **A commenter (Matias)** reports using it to debug agents on Qwen models: missing context shows as "need info" concepts, while an execution slip leaves the internal state looking "healthy". Anecdotal, no numbers or controls.
> - **Possible uses here (Claude's suggestions):** check whether privacy-related concepts are active before a leak (knew vs did not know); redo the logit-lens analysis of [[goel-2026-privacy-collapse]] before and after fine-tuning; swap the recipient concept to test whether the disclosure decision depends on it.
> - **Limits stated in the post:** needs open weights; concepts must fit roughly one token; explains at most about 10% of activation variance beyond a random control; concept swaps worked in 76 of 192 trials.
>
> **Original post:** [A global workspace in language models](https://www.anthropic.com/research/global-workspace) (Anthropic Interpretability team, 6 Jul 2026). Read on 2026-11-15 from the page text; figures and the linked technical paper were not read. It does not mention privacy.
> - **Monitoring section, as described:** the same word-level readout is used on four cases. In a blackmail test scenario Claude Sonnet 4.5 does not blackmail, but words like "fake" and "fictional" appear internally; switching those patterns off led it to threaten blackmail "some of the time". A model that edited a score file directly showed "manipulation". A model trained to write subtly sabotaged code showed "secretly" and "fraud" on ordinary coding requests, "even where its output looks unremarkable".
> - **Not a working monitor:** no detection or false-positive rates, no thresholds, no comparison with other methods. The authors call these "a few examples" and the lens "an imperfect tool".
> - **Also reported:** after post-training, warning words appear while the model reads a risky request; in the pretrained model they appear only when it starts writing.
>
> **Research ideas from the discussion (Claude's suggestions, 2026-11-15; novelty not checked):**
> 1. **Do privacy benchmarks measure test-taking?** All benchmarks in this wiki are synthetic ([[pisas]], [[cimemories]], [[agentcibench]], PrivacyLens). On an open model, check whether evaluation-awareness words appear on benchmark prompts, then switch them off and see whether leakage changes.
> 2. **Detecting a collapsed or backdoored model from the inside.** Compare a base model with its "helpful" fine-tune ([[privacy-collapse]]) on the same prompts: do privacy-related words stop appearing after collapse? This would not depend on finding a backdoor trigger. Suggested starting point.
> 3. **Reading vs writing.** Does a careful model flag a sensitive attribute when it first reads it, and a leaky one only at writing time or never? A concrete measurement for "knew vs did not know".
> 4. **Monitoring internal channels.** Check each agent internally before it sends a message, as an alternative to output filters, which cost completeness in [[gupta-2026-pisas]]. Most speculative.
> - **Limits for all four:** open weights only, and a fine-tuned model needs its own lens fit; single-word proxies for a norm that has no single token; a null result is hard to interpret because the lens covers under a tenth of the model's activity.

## Open questions

- Does an agent that picks its own tools and context behave like the "helpful" fine-tuned models in [[goel-2026-privacy-collapse]]? Hamid's question 2 in `my-notes/questions.md`; see [[privacy-collapse#Open questions]].
- Why do prompting defenses help here while [[mireshghallah-2025-cimemories|CIMemories]] reports a trade-off for memory? See [[prompt-defenses-for-contextual-privacy#Free lunch or trade-off]].
