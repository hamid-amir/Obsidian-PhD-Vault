---
type: source
title: "Hide and Seek in Embedding Space: Geometry-based Steganography and Detection in Large Language Models"
tags: [fine-tuning, defenses]
authors:
  - Charles Westphal
  - Keivan Navaie
  - Fernando E. Rosas
venue: arXiv
year: 2026
status: seed
url: https://arxiv.org/abs/2601.22818
created: 2026-11-19
updated: 2026-11-19
---

# Hide and Seek in Embedding Space

> [!warning] Seed page
> Metadata only, not yet ingested. Add `zotero`, and write the summary, when ingesting.

Fine-tuned LLMs that covertly encode a secret from the prompt into fluent outputs: vocabulary buckets derived from random hyperplanes in token-embedding space replace TrojanStego's token-ID parity, lowering how recoverable the payload is to a keyless observer. On the defense side, argues distribution-shift steganalysis cannot separate this from ordinary fine-tuning drift and proposes linear probes on later-layer hidden states. arXiv 2601.22818 (cs.CR, cs.AI).

## Where this fits

- [[privacy-collapse]]
- [[rose-2026-multi-agent-collusion-detection]]
