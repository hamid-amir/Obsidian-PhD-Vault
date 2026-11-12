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
