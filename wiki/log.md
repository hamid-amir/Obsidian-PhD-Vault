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
