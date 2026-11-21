---
type: source
title: 'Breaking Bad: "Going Badly" Activations Drive Reward Hacking'
tags:
  - interpretability
  - agents
authors:
  - Natalia Fischl-Lanzoni
  - Punya Syon Pandey
  - Merlin Stein
  - David Demitri Africa
venue: arXiv
year: 2026
status: seed
url: https://arxiv.org/abs/2611.14996
created: 2026-11-21
updated: 2026-11-21
---

# Breaking Bad: "Going Badly" Activations Drive Reward Hacking

> [!warning] Seed page
> Metadata only, not yet ingested. Add `zotero`, and write the summary, when ingesting.

Builds a "task badness" direction from model-written stories about everyday tasks going well vs badly (no hacking in the stories), reads it along coding-agent runs where it rises before reward hacks, and steers against it to cut hacking in Qwen3.6-27B; effects are weaker or inconsistent in GPT-OSS-20B and Gemma-4-31B. Strong steering makes the model misreport failing tests as passing. Not about privacy. arXiv 2611.14996 (cs.CL, cs.AI).

## Where this fits

- [[activation-probing-and-steering]]
