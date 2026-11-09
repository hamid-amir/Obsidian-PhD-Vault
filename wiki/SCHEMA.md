# Wiki Schema

This file is the configuration for this wiki. It documents the conventions, page types, tag taxonomy, and any workflow customizations. The LLM reads this first when entering the wiki, and its conventions override the defaults documented in the `llm-wiki` skill.

This file is **co-evolved with the user**. When the LLM notices a recurring pattern in your edits or feedback that isn't here, it will propose adding it. When something here stops fitting, prune it.

## Wiki location

Paths are relative to the vault root (the Obsidian vault is the project root).

- Wiki root: `wiki/`
- Raw sources: **Zotero is the primary raw store for papers.** PDFs stay in Zotero and are never copied into the vault (keeps git small). Source pages point to them through `zotero` / `url`.
- `raw/` holds only material Zotero can't: web clippings, transcripts, talk notes. Immutable once added.
- `raw/zotero/<citekey>.md` holds Hamid's highlights and comments, imported from Zotero by the Zotero Integration plugin (template: `templates/zotero-annotations.md`). They are regenerated on re-import, so never edit them. Read them during ingest: they show what Hamid found important, and his comments (marked **Hamid:**) are his own views, not the paper's claims. Images from annotations go in `raw/assets/zotero/`.
- Asset/image storage: `raw/assets/`
- Human layer: `my-notes/` is written by Hamid only. Read it for context and link to it, but never edit, move or rewrite it.

## Page types

This wiki uses these page types, each with a dedicated subdirectory:

- `source` (in `wiki/sources/`) — one summary page per ingested source.
- `entity` (in `wiki/entities/`) — pages about specific things: people, papers, products, places, organizations.
- `concept` (in `wiki/concepts/`) — pages about ideas, methods, frameworks, abstractions.
- `synthesis` (in `wiki/synthesis/`) — cross-cutting analyses, comparisons, query answers filed back.

Add additional types here as the wiki evolves.

## Tag taxonomy

Keep this list small. Propose a new tag to Hamid before using it.

- `contextual-integrity` — CI theory and work that applies it.
- `agents` — LLM agents, including computer-use agents.
- `llm-memory` — persistent memory and personalization in LLMs.
- `fine-tuning` — effects of training or fine-tuning on privacy behavior.
- `benchmark` — evaluation datasets and harnesses.
- `defenses` — mitigations (prompting, training, system-level).
- `open-question` — pages or sections that flag unresolved questions.
- `contested` — pages where sources contradict.
- `stub` — page exists but has little content yet.

## Page sizing

- Soft cap: 400 lines / ~2,000 words. Consider splitting beyond this.
- Hard cap: 800 lines. Must split.

## Frontmatter requirements

Every page must have:
- `type`
- `title`
- `tags`
- `created`
- `updated`

Plus type-specific:
- `source` pages: `authors` (full list), `venue` (plain text, not a link), `year` (number), `status`, plus `url`, `zotero` (Better BibTeX citekey or `zotero://` link) and `ingested` (date) once ingested. `raw` only when the source lives in `raw/`.
- Non-source pages: `sources` listing the source-summary pages drawn from (bare slugs).

Source `status` values:
- `seed` — created from metadata or Hamid's notes, not yet read from the full text. Claims are unverified.
- `ingested` — summarized from the full text by the ingest workflow.

Naming: source slugs are `<first-author-surname>-<year>-<short-title>`, e.g. `goel-2026-capable-but-careless`. All slugs are lowercase-hyphenated and unique across the vault (Obsidian resolves `[[links]]` by filename). Never rename a file by changing case only: the Mac copy of this vault is on a case-insensitive filesystem.

## Optional graph metadata

Pages may declare typed graph metadata under a top-level `graph:` key. This is the source of truth for the compiled knowledge graph under `wiki/graph/`. Markdown remains canonical; the graph is a regenerable index. Pages without `graph:` still appear as nodes (derived from `type`/`kind`) and still contribute `mentions` edges from body `[[wikilinks]]`.

```yaml
graph:
  node_id: person:praney-behl       # optional; default <node_type>:<slug>
  node_type: person                  # optional; default mapped from type/kind via ontology
  canonical: true                    # mark as canonical when multiple slugs alias the same entity
  aliases: [Praney, praney@example.com]
  relationships:
    - predicate: founded
      object: company:seedblocks
      source: praney-founder-context-dump   # source-page slug
      evidence: "Solo technical founder and sole director..."
      confidence: high               # high | medium | low
      status: current                # current | historical | proposed | disputed | superseded
      # optional:
      # valid_from: 2025-01-15
      # valid_to: 2026-03-01
      # notes: "..."
      # raw_ref: "raw/founder-dump.md#L42"
      # contradicts: edge-id-or-source-slug
      # supersedes: edge-id-or-source-slug
```

Required fields on every relationship: `predicate`, `object`, `source`, `evidence`, `confidence`, `status`. Predicates and the subject/object types they accept are declared in `wiki/graph/ontology.yaml`. Typed semantic edges must be supported by an explicit source — never emit one inferred from training data alone.

## Index structure

(Update this section when sharding.)

Currently flat: a single `wiki/index.md` listing all pages.

When the wiki passes ~150 pages or `index.md` exceeds 300 lines, shard into `wiki/indexes/<type>.md` and update this section.

## Retrieval

- Search is section-level hybrid by default: `uv run --script skills/llm-wiki/scripts/wiki_search.py "query" --json`.
- Semantic backend: local FastEmbed + sqlite-vec (`BAAI/bge-small-en-v1.5`, 384 dimensions). No wiki or query text leaves the machine.
- First semantic use downloads model artifacts to `~/.cache/llm-wiki/fastembed/`; set `FASTEMBED_CACHE_PATH` to override the model cache.
- `wiki/.wiki-cache/` holds regenerable retrieval artifacts: `search-index.json` (parse cache) and `embeddings.sqlite` (section metadata + sqlite-vec vectors). Safe to delete; never edit by hand; gitignored.
- The vector index is content-hashed: only new or changed sections are re-embedded, deleted sections are removed, and model/schema changes rebuild it automatically.
- Dependency-free lexical path: `python skills/llm-wiki/scripts/wiki_search.py "query" --no-embed` (direct Python bypasses PEP 723 dependency resolution). A missing or failed local backend also falls back to lexical search without failing the command.

## Graph layer

The wiki has an optional compiled graph layer under `wiki/graph/`:

- `wiki/graph/ontology.yaml` — declares node types and predicates. **Tracked.** Edit this when you introduce new predicates or domain types.
- `wiki/graph/nodes.jsonl`, `wiki/graph/edges.jsonl` — generated. Track in git only if you want graph diffs in PRs.
- `wiki/graph/graph.sqlite` — generated. Gitignored by default.
- `wiki/graph/graph.graphml` — generated. Track only if you want to diff it.

Generation is reproducible from markdown via `scripts/wiki_graph_extract.py`. The graph can be deleted at any time and rebuilt without losing knowledge — markdown is canonical.

## Workflow customizations

- Ingest from Zotero: read the paper itself (arXiv URL or the PDF in Zotero, in chunks), plus Hamid's annotations in `raw/zotero/<citekey>.md` if present. Use the Better BibTeX citekey as the `zotero` field, and link the annotations file as `raw`. Do not copy the PDF into the vault.
- Upgrading a `seed` page: re-read the full text, verify every claim and number already on the page, keep Hamid's takeaways (label them as his notes), then set `status: ingested` and `ingested: <date>`.
- `Papers.base` (vault root) is an Obsidian Bases table over `wiki/sources/`. Keep source frontmatter keys consistent so it renders.

## User preferences

- Use Obsidian callouts (`> [!note]`, `> [!caution]`, …) and `==highlights==`. Never use HTML tags such as `<font color>`.
- Hamid's own takeaways are worth keeping verbatim; mark them as his notes rather than rewriting them in wiki voice.

## Lint cadence

- Structural lint: after every 5 ingests.
- Semantic lint: weekly or after every 20 ingests.
- Gap-finding: monthly.
- Graph lint + extract: after every ingest that adds typed `graph.relationships`.

Adjust based on the wiki's growth rate.

## Skill evolution

Optional: capture verified task experience with `/wiki:learn` and propose tested procedural improvements with `/wiki:evolve`. Existing pages and workflows do not change. Raw experience JSON lives under the configured raw root's `experiences/` directory and is immutable. Consolidate observations into ordinary source/concept pages with evidence, applicability, model/tool versions and counterexamples. Optional pattern fields are `kind: experience-pattern` and `status: hypothesis|supported|superseded`.

`wiki/.evolution/` holds durable experiment snapshots, evidence, diffs and measured outcomes; it is not a cache. Wiki search, lint, stats and graph extraction exclude it. Keep searchable summaries in normal synthesis pages linked to the source/pattern pages and index. Private records must not be published with shared skills.

Candidate edits remain isolated until reviewed, selected on validation tasks, independently measured on untouched final-test tasks, and explicitly applied within the user's authorization. Failed edits leave the active skill unchanged while their records persist. Do not repeat a rejected proposal without new evidence or changed conditions. Keep factual wiki access during normal work; do not inject experiment history into task execution. Correct unsupported lessons without deleting their raw evidence. Follow the installed skill's `references/evolution-workflow.md` for the runner contract, cost limits and recovery.
