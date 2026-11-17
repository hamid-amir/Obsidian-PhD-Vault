# Wiki Log

Append-only chronological record of operations on the wiki. Each entry begins with `## [YYYY-MM-DD] <op> | <description>` so it's parseable with `grep "^## \[" log.md | tail -N`.

Operations:
- `ingest` — a source was processed into the wiki.
- `query` — a question was answered against the wiki (typically only logged when the answer was filed back as synthesis).
- `lint` — a health check was run.
- `schema` — the schema was modified.
- `shard` — an index was sharded.

---

## [2026-11-09] schema | Vault restructured into the LLM-wiki layout; Notes/ and Sources/ migrated as seed pages (wording kept), questions moved to my-notes/, CLAUDE.md added

## [2026-11-09] schema | Added Zotero Integration import: annotations land in raw/zotero/<citekey>.md via templates/zotero-annotations.md

## [2026-11-09] ingest | Capable but Careless (AgentCIBench), arXiv 2606.23189 — seed upgraded to ingested from full text; new [[agentcibench]], [[prompt-defenses-for-contextual-privacy]]; updated privacy-leakage-in-agents, privacy-leakage-in-llm-memory, contextual-integrity, goel-2026-privacy-collapse. No Zotero annotations found; zotero citekey not set.

## [2026-11-12] ingest | Privacy Collapse, arXiv 2601.15220 — seed upgraded to ingested from arXiv HTML text (figures not read); new [[privacy-collapse]], backdoored-privacy-collapse-what-it-shows (synthesis, later removed); updated privacy-leakage-in-agents, privacy-leakage-in-llm-memory, contextual-integrity, index. No Zotero annotations found; zotero citekey not set. No typed graph edges added.

## [2026-11-12] ingest | Removed synthesis page backdoored-privacy-collapse-what-it-shows at Hamid's request; links to it removed from goel-2026-privacy-collapse, privacy-collapse, privacy-leakage-in-agents, index.

## [2026-11-12] ingest | CIMemories, arXiv 2511.14937 — seed page [[mireshghallah-2025-cimemories]] created with verified metadata only; linked from privacy-leakage-in-llm-memory; index updated.

## [2026-11-12] ingest | CIMemories, arXiv 2511.14937 — seed upgraded to ingested from arXiv HTML text (figures, Appendix B and low prompt not read); new [[cimemories]]; updated privacy-leakage-in-llm-memory (stub tag removed), prompt-defenses-for-contextual-privacy (contradiction with AgentCIBench recorded, contested tag), privacy-leakage-in-agents, contextual-integrity, privacy-collapse, agentcibench, both Goel source pages (links only), index. No Zotero annotations found; zotero citekey not set. No typed graph edges added.

## [2026-11-12] ingest | PiSAs, arXiv 2607.05318 — seed page [[gupta-2026-pisas]] created with verified metadata only; index updated.

## [2026-11-14] ingest | PiSAs, arXiv 2607.05318 — seed upgraded to ingested from the v2 PDF text (19 Oct 2026; Appendices H–P and figures not read). v1 numbers discarded. New [[pisas]]; updated privacy-leakage-in-agents (settings map, evidence), privacy-leakage-in-llm-memory, prompt-defenses-for-contextual-privacy, contextual-integrity, cimemories, index. No Zotero annotations found; zotero citekey not set. No typed graph edges added.

## [2026-11-14] ingest | Why Do Naive SFT Filters For Safety Properties Fail? (Engels & Nanda, LessWrong) — paraphrased clipping saved to raw/engels-2026-naive-sft-filters.md; new source page engels-2026-naive-sft-filters (figures not read; page later removed); open question added to privacy-collapse; index updated.

## [2026-11-15] ingest | Side note on the J-lens blog post (Louapre, Hugging Face) added to privacy-leakage-in-agents at Hamid's request; no source page or raw clipping created.

## [2026-11-15] ingest | Removed source page engels-2026-naive-sft-filters at Hamid's request; content kept as a side note on privacy-collapse; raw clipping left in place; index updated.

## [2026-11-15] ingest | J-lens side note on privacy-leakage-in-agents extended with the original Anthropic post (monitoring section) and four research ideas, at Hamid's request; no source page created.

## [2026-11-16] query | New entity page [[benchmarks-at-a-glance]]: summary table of AgentCIBench, CIMemories and PiSAs built from their entity and source pages, at Hamid's request; to be extended on every benchmark ingest. Linked from agentcibench, cimemories, pisas and index.

## [2026-11-16] ingest | AgentLeak, arXiv 2602.11510 — seed page [[el-yagoubi-2026-agentleak]] created with verified metadata only; index updated.

## [2026-11-16] ingest | AgentLeak, arXiv 2602.11510 — seed upgraded to ingested from v3, read through section-by-section model extractions of the arXiv HTML/PDF text (not a direct read; figures not read), with focus on contextual-privacy relevance at Hamid's request. New [[agentleak]], [[internal-agent-flows-exposure-or-harm]] (synthesis, from the discussion); updated privacy-leakage-in-agents (evidence, settings map, failure modes, open question), prompt-defenses-for-contextual-privacy, privacy-leakage-in-llm-memory, contextual-integrity, benchmarks-at-a-glance (new row), pisas, gupta-2026-pisas (one line), index. No Zotero annotations found; zotero citekey not set; venue left as arXiv (IEEE Access DOI unconfirmed). No typed graph edges added.

## [2026-11-16] schema | At Hamid's request: SCHEMA.md gains rules for the benchmark table (update on every benchmark ingest, fixed short columns), latest paper version only, shorter and more focused pages, no internal-inconsistency sections and no how-it-was-read notes (these go in the log). Applied to the wiki: removed the inconsistency callouts and version/reading notes from all five ingested source pages and the lines pointing to them on entity and concept pages; benchmarks-at-a-glance pruned to eight columns (paper, models tested and caveats dropped; maintenance note and 'mentioned but no page yet' list removed). Reading provenance removed from pages: AgentLeak was read through model extractions of the v3 text (figures not read); earlier ingests as recorded in their log entries above.

## [2026-11-17] ingest | Contextual Integrity in LLMs via Reasoning and Reinforcement Learning, arXiv 2506.04245 — seed page [[lan-2025-ci-reasoning-rl]] created with verified metadata only (authors and NeurIPS 2025 from arXiv and the NeurIPS poster page); linked from prompt-defenses-for-contextual-privacy and privacy-collapse (open questions); index updated.

## [2026-11-17] schema | At Hamid's request, my-notes/general-meetings.md and my-notes/questions.md merged and lightly reworded into my-notes/research-agenda.md (question numbers kept); references to my-notes/questions.md in privacy-collapse, prompt-defenses-for-contextual-privacy and privacy-leakage-in-agents, and the questions link in my-notes/multi-agent scenario.md, now point to it. Old files left for Hamid to delete.

## [2026-11-17] schema | At Hamid's request, every "Open questions" section was removed from the wiki (privacy-collapse, privacy-leakage-in-agents, privacy-leakage-in-llm-memory, prompt-defenses-for-contextual-privacy, internal-agent-flows-exposure-or-harm) and merged into my-notes/research-agenda.md: duplicates of Hamid's questions 1, 2, 3, 5 and 6 folded in as wiki context, the rest added under "Open questions from the wiki". Each page's Related list now links [[research-agenda]]; open-question tag dropped from prompt-defenses-for-contextual-privacy and internal-agent-flows-exposure-or-harm; index line for the synthesis page trimmed.
