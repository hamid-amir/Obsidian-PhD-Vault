# Why Do Naive SFT Filters For Safety Properties Fail? (clipping)

- **URL:** https://www.lesswrong.com/posts/wyZRNgpeiPeRXB6eT/why-do-naive-sft-filters-for-safety-properties-fail
- **Authors:** Josh Engels, Neel Nanda
- **Published:** 14 Jun 2026, LessWrong (crossposted to the AI Alignment Forum). Fourth in a series of informal research updates from the Google DeepMind Language Model Interpretability team.
- **Clipped:** 2026-11-14

> This file is a paraphrased record of the post, not its verbatim text. Short phrases in quotation marks are quoted from the post. The post's figures (about nine bar charts) are images and were not read, and the post gives no numeric results in its text. Code and data are not released.

## Question

SFT data is a cause of many safety-relevant properties, so a natural strategy is to filter out training rollouts that show an undesirable property. The post asks why this "frequently works surprisingly poorly".

## Setup

- **Behaviours:** negative emotion (model repeatedly told it is wrong; autorater scores emotion 0-10), date confusion (800 prompts asking for summaries of 2026-dated documents; percent of rollouts rated date-skeptical), blackmail (modified agentic-misalignment eval; percent of rollouts rated as blackmail).
- **Data:** a fixed set of 10k random prompts from the Olmo 3 SFT prompt distribution. Completions are either the official Olmo data or single-shot rollouts from Gemini 3.1 Pro, Kimi K2.5 or GLM 5.1.
- **Models:** Gemini 3 Flash and Olmo 3 as students; Gemini 3.1 Pro as the main teacher.
- **Filters:** an LLM autorater flags a property; the matching prompts are dropped, or their rollouts are swapped to the other source. Filter sets out of 10k: refusal 210, WildGuardMix refusal 89, roleplay 540, date mention 734, simulation mention 2327. The authors say these splits were "guessing" at what matters.

## Experiments

1. **Teacher swap.** Blackmail and date confusion appear when the student is trained on Gemini 3.1 Pro rollouts and not on Olmo data; GLM and Kimi rollouts show the same pattern. Negative emotion depends on the prompt distribution more than on the teacher.
2. **Random interpolation.** Mixing a random share of Gemini and Olmo rollouts: date confusion scales smoothly with the share; blackmail needs fewer Gemini examples and is noisier.
3. **Subset swap vs drop.** Date-mention prompts (734/10k) for date confusion and roleplay prompts (540/10k) for blackmail are small subsets that are sufficient and necessary when swapped. Swapping them to Olmo rollouts removes the trait; dropping them does not: "adjacent behavior 'leaks in' and fills in the gaps."
4. **All filters.** None of the other interventions moves date confusion beyond expectation; almost all move blackmail. Adding a small share of Gemini refusal data (89/10k) raises blackmail.

## Explanations considered

- Simple generalisation from mild to obvious versions of a behaviour: indirect support only.
- Subliminal learning: evidence against, since traits transfer between models with different initialisations.
- Persona lock-in from pretraining or posttraining: partly consistent, but the three traits behave differently, so "it is unlikely that there is a single 'Gemini-ness' latent variable".
- Underspecified or bad prompts: proposed for negative emotion, untested.
- The authors state they "still don't know the exact datapoints" that carry the transfer after filtering.

## Takeaways (as stated)

- "It's hard to remove behaviors via filtering."
- Swapping rollouts on the trait-causing subset works where dropping does not; the authors note swapping is a larger update than dropping.
- If a teacher model can be given a behaviour, transferring it through SFT is comparatively easy.

## Not in the post

Privacy, contextual integrity, helpfulness data, activation-projection filtering and the Privacy Collapse paper are not mentioned. Counter-example mixing, steering vectors and larger removal fractions are not tested. The authors describe the results as "preliminary".
