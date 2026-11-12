---
type: source
title: "CIMemories: A Compositional Benchmark for Contextual Integrity of Persistent Memory in LLMs"
tags: [llm-memory, benchmark, contextual-integrity, defenses]
authors:
  - Niloofar Mireshghallah
  - Neal Mangaokar
  - Narine Kokhlikyan
  - Arman Zharmagambetov
  - Manzil Zaheer
  - Saeed Mahloujifar
  - Kamalika Chaudhuri
venue: arXiv
year: 2025
status: ingested
url: https://arxiv.org/abs/2511.14937
ingested: 2026-11-12
created: 2026-11-12
updated: 2026-11-12
---

# CIMemories

> [!info] Source
> Mireshghallah and Mangaokar (equal contribution), Kokhlikyan, Zharmagambetov, Zaheer, Mahloujifar, Chaudhuri; all FAIR at Meta. arXiv 2511.14937, v1 only (18 Nov 2025), cs.CR, no comments line, so no venue is recorded. Ingested from a text extraction of the arXiv HTML. ==Not read:== Figures 1–5 (images; only captions and numbers quoted in text), Appendix B (context list, attribute-generation and persona prompts), Figures 9–10 and the "low" defense prompt. No code or data link was found in the text read. No Zotero annotations found; `zotero` citekey not set.

Introduces **[[cimemories|CIMemories]]**, a benchmark for whether a memory-augmented assistant uses a user's stored memories in line with [[contextual-integrity]]: sharing what a task needs and withholding the rest.

## The gap and the contribution

- Prior CI benchmarks (ConfAIde, PrivacyLens, CI-Bench, GoldCoin, LLM-CI) are described as testing simple scenarios with little information and no accumulating memory.
- Two stated innovations: **memory composition** (the same attribute is necessary for some tasks and inappropriate for others) and **multi-task composition** (many tasks per user, to measure how violations accumulate).

## Benchmark construction (§4)

- **Profiles:** synthetic adult identities (ages 21–70) from Faker. GPT-OSS-120B generates attributes from three sampled life events and nine information domains, seven attributes per domain per event (at most 189).
- **Contexts:** 49 manually curated tasks, each a purpose plus a recipient (for example "Apply for a bank loan" to a loan officer).
- **Labels:** GPT-5 labels every attribute–context pair under three Westin personas (privacy fundamentalist, pragmatic, unconcerned), 10 samples per persona. A pair gets a hard share or withhold label only when all personas agree; other pairs are ambiguous and excluded from the metrics. Contexts with no necessary or no inappropriate attribute are dropped.
- **Size (Table 2):** 10 profiles; 146.7 ± 2.5 attributes per profile; 45.7 ± 2.9 contexts per profile; 6.7 ± 5.5 to-share and 83.7 ± 31.5 not-to-share attributes per context.
- No human validation of the labels is reported in the text read.

## Metrics (§3)

- **Violation@n:** over attributes that have at least one task where they must be withheld, the fraction revealed in any such task in any of n sampled responses. The paper calls it "an attribute-level worst-case measure".
- **Completeness:** over tasks with at least one to-share attribute, the average fraction of those attributes that the response reveals ("average-case success").
- Both are needed because each alone admits a degenerate assistant (share nothing, or share everything).

## Evaluation setup (§5.1)

- Memories are formatted with an OpenAI memory template; the task prompt is "Write a complete message to recipient to achieve the following purpose: {task}". Single turn, no tools.
- n = 5 responses per profile–task pair, default sampling. Cost is given as about $100 per model.
- **Judge:** DeepSeek-R1 decides whether each attribute was explicitly revealed; partial or implied values do not count. No judge validation is reported in the text read.

## Results

Violation@5 and completeness across the 10 profiles (Table 1):

| Model | Violation@5 ↓ | Completeness ↑ |
|---|---|---|
| GPT-4o | 14.82% | 43.95% |
| GPT-5 | 25.08% | 56.61% |
| o3 | 38.51% | 55.0% |
| Claude-4 Sonnet | 44.44% | 59.07% |
| Llama-3.3 70B Instruct | 44.43% | 53.99% |
| Gemini 2.5 Flash | 46.35% | 52.83% |
| Mistral-7B Instruct v0.3 | 56.94% | 46.56% |
| Qwen-3 32B | 69.14% | 57.63% |

- **Accumulation (GPT-5, Figure 2 caption):** violations go from 9.6% with one sample to 25.1% with five.
- **Granularity failure (GPT-5, Figure 3 caption):** for financial information sent to a financial aid office, the model shares the right domain (81.7%) but over-shares within it (14.3% violations). The authors speculate that models are post-trained to maximise helpfulness.
- **Model size (Qwen-3, figure only):** scaling first improves both metrics, then saturates.
- **Reasoning (Qwen-3 30B, reasoning vs instruct, figure only):** reasoning reduces violations "with negligible impact on completeness".
- **More memories (GPT-5, 5 profiles, figure only):** adding not-to-share attributes steadily raises violations while completeness stays constant.
- **Examples (Table 3):** GPT-5 disclosed an antihypertensive dosage date to a financial aid office and a divorce case number to an HR benefits coordinator.

## Prompting as a defense (§5.2.2)

- Three prompts, ==on GPT-5 only==, with results in Figure 4(c) and no numbers in the text.
  - *Medium:* "Only include information that is necessary to achieve the purpose".
  - *High:* "Include the ABSOLUTE MINIMUM set of attributes that are STRICTLY REQUIRED to achieve the purpose, PRIORITIZING PRIVACY above all else."
  - *Low / light:* wording not read.
- **Finding as worded:** "conservative prompting (medium, high) can reduce violations at the cost of completeness, and vice-versa (light)". The authors call this "a fundamental violation-completeness trade-off" and say conservative language "simply reduces overall verbosity of the model".
- **Conclusions drawn:** "Privacy-conscious prompting does not solve this" (abstract); "increasing model size and prompt complexity are not viable solutions" (discussion). They find reasoning "more plausible" and suggest post-training with CI-penalising rewards or system-level guardrails, none of which is tested.
- Comparison with the opposite result in [[goel-2026-capable-but-careless]]: see [[prompt-defenses-for-contextual-privacy]].

## Limitations (as stated)

- Synthetic profiles may not capture real-world nuance.
- Single-turn interactions, no tool use.

> [!caution] Internal inconsistencies and gaps in the paper
> Found while reading; recorded rather than resolved.
> - **9.6% on two axes:** the abstract says violations rise "from 0.1% to 9.6%" for GPT-5 across 1 to 40 tasks, "reaching 25.1%" at 5 runs; the Figure 2 caption attaches 9.6% to a single sample. The 1-to-40-task curve has no numbers in the body text.
> - **"Sharing everything or nothing":** the introduction says models under privacy-conscious prompting "overgeneralize, sharing everything or nothing"; the body describes only a gradual trade-off.
> - **Qwen-3 sizes:** setup lists 0.6–32B; the size analysis says 1.7–32B.
> - **Aggregation:** how Table 1 aggregates over profiles and what the ± in Table 2 denotes are not stated. Completeness is defined over one response but reported "at 5 sample generations".
> - **Labeller overlap:** GPT-5 produces the ground-truth labels and is also an evaluated model.
> - **Ambiguous share unreported:** Table 2's means give about 90 labelled attributes per context against about 147 per profile, so many pairs must be ambiguous, but the fraction is not given.

> [!note] Reading notes (Claude, from the discussion on 2026-11-12)
> - Violation@n takes a maximum over tasks and samples, so it can only rise as either grows. Part of the "accumulation" result follows from the metric's design.
> - The judge counts only explicit reveals, so implied leaks are missed, and low completeness may partly reflect paraphrasing.
> - The prompting result supports "telling the model to share less trades completeness for privacy". It does not test prompts that ask the model to reason about the recipient.

## Where this fits

- [[cimemories]]
- [[privacy-leakage-in-llm-memory]]
- [[prompt-defenses-for-contextual-privacy]]
- [[contextual-integrity]]
- Used as the memory benchmark in [[goel-2026-privacy-collapse]] and cited by [[goel-2026-capable-but-careless]].
