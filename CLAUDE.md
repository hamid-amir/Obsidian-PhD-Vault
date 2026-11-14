# PhD Vault: instructions for Claude

This is Hamidreza (Hamid) Amirzadeh's PhD research vault (INSAIT). It is an Obsidian vault run as an **LLM wiki** following Andrej Karpathy's pattern: Hamid curates sources and asks questions; Claude compiles sources into a persistent, interlinked wiki and keeps it consistent. Knowledge should compound, not be re-derived from raw papers each session.

**Research focus:** contextual privacy in LLMs and LLM agents, studied through contextual integrity (CI): when models and agents disclose information inappropriately, in agent actions, in persistent memory and after fine-tuning, and which defenses work.

## Layout

```
CLAUDE.md          this file
Papers.base        Obsidian Bases table over wiki/sources/
my-notes/          HAMID'S OWN WRITING. Read for context, link to it, never edit/move/rewrite.
raw/               immutable non-Zotero sources (clippings, transcripts); raw/assets/ for images
  zotero/          Hamid's Zotero highlights + comments per paper (auto-generated, never edit)
templates/         Obsidian templates (Zotero import template)
wiki/              CLAUDE-OWNED wiki
  SCHEMA.md        conventions: page types, frontmatter, tags, naming. AUTHORITATIVE.
  index.md         catalog, one line per page. Read this first.
  log.md           append-only operation log
  sources/         one page per paper/source
  concepts/        ideas, methods, frameworks
  entities/        specific people, benchmarks, models, organizations
  synthesis/       cross-cutting analyses, comparisons, answers filed back
  graph/           optional compiled graph layer (ontology.yaml tracked, rest generated)
```

Paper PDFs live in **Zotero**, not in the vault (`~/Zotero/` on Linux). Source pages reference them through `zotero` / `url`.

## How to work here

Use the **`llm-wiki`** skill for every wiki operation. Read `wiki/SCHEMA.md` before writing; it overrides the skill's defaults.

- **Query:** read `wiki/index.md` first, open only the relevant pages, answer with `[[wikilink]]` citations. If the wiki lacks coverage, say so; don't answer from training data as if it came from the wiki. Offer to file good answers into `wiki/synthesis/`.
- **Ingest:** a source page in `wiki/sources/`, surgical updates to the concept/entity pages it touches, new pages linked from existing ones (no orphans), a line in `index.md` and an entry in `log.md`.
- **Lint:** run the skill's `wiki_lint.py` on `wiki/` and present findings as proposals. Don't apply them silently.

## Rules

1. **Never invent.** Every claim in a wiki page must trace to a source page. Hedge uncorroborated claims, record contradictions instead of resolving them silently, and never add metadata (authors, URLs, numbers) you have not verified.
2. **`status: seed` pages are unverified.** They were written from Hamid's notes or metadata only. Upgrade them to `status: ingested` only after reading the full text, keeping Hamid's takeaways labeled as his.
3. **Surgical edits.** Change the relevant section only; never rewrite whole pages. Keep pages atomic (400-line soft cap).
4. **Links and frontmatter.** Body links are `[[slug]]` or `[[slug|text]]`. Frontmatter lists use bare slugs. Every wiki page has `type, title, tags, created, updated` (+ `sources` on non-source pages). Use only tags listed in SCHEMA.md.
5. **Obsidian formatting.** Callouts and `==highlights==` are fine. No HTML tags (`<font>`, etc.).
6. **Ask before** restructuring folders, renaming pages, adding tags or editing `my-notes/`, `raw/`, `CLAUDE.md` or `SCHEMA.md`.

## Sync between two machines

The vault is a git repo (`github.com:hamid-amir/Obsidian-PhD-Vault`), synced by the Obsidian Git plugin on a Linux machine (`/home/hamid/PhD-Vault`) and a Mac (`/Users/hamidreza/PhD Vault`). Both auto-commit and push every few minutes.

- Assume the other machine may have pushed: work from the current files on disk, never from an old copy.
- The Mac filesystem is case-insensitive: never create two paths that differ only by case, and never rename by case alone.
- Don't commit machine-local files (`.obsidian/workspace*.json`, `.DS_Store`, `wiki/.wiki-cache/`).
