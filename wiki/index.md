# Wiki Index

The catalog of all pages in this wiki. Each entry: a wikilink to the page and a one-line summary. The LLM reads this first when answering queries to identify candidate pages.

Keep summaries tight — one line each. The index is engineered to be cheap to read; a fat index defeats its purpose.

When this file exceeds ~300 lines or the wiki passes ~150 pages, shard into `wiki/indexes/<type>.md` and replace this file with a directory of shards. See the `scaling-playbook.md` reference in the `llm-wiki` skill for the migration procedure.

---

## Sources

- [[goel-2026-capable-but-careless]] — AgentCIBench: computer-use agents leak without an attacker; capability ≠ carefulness (EMNLP 2026, seed)
- [[goel-2026-privacy-collapse]] — benign fine-tuning can break contextual privacy in LLMs (ACL 2026, seed)
- [[nissenbaum-2004-privacy-as-contextual-integrity]] — foundational paper defining contextual integrity (Washington Law Review 2004, seed)

## Entities

(populated as entity pages are created)

## Concepts

- [[contextual-integrity]] — privacy as appropriate information flow; the five flow parameters
- [[privacy-leakage-in-agents]] — agents leak by their own judgment; fine-tuning can worsen it; cheap prompting helps
- [[privacy-leakage-in-llm-memory]] — stub: can models use persistent memory appropriately?

## Synthesis

(populated as query answers are filed back)
