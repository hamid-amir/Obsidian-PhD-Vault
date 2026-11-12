# Wiki Index

The catalog of all pages in this wiki. Each entry: a wikilink to the page and a one-line summary. The LLM reads this first when answering queries to identify candidate pages.

Keep summaries tight — one line each. The index is engineered to be cheap to read; a fat index defeats its purpose.

When this file exceeds ~300 lines or the wiki passes ~150 pages, shard into `wiki/indexes/<type>.md` and replace this file with a directory of shards. See the `scaling-playbook.md` reference in the `llm-wiki` skill for the migration procedure.

---

## Sources

- [[goel-2026-capable-but-careless]] — AgentCIBench: 15 computer-use agents leak without an attacker (mean 67.9%); capability ≠ carefulness; prompt defenses cut leakage ~35 pts (EMNLP 2026)
- [[goel-2026-privacy-collapse]] — benign fine-tuning (helpful, empathetic, support data) breaks contextual privacy while safety/capability hold; can be backdoored; data filtering helps partly (ACL 2026)
- [[nissenbaum-2004-privacy-as-contextual-integrity]] — foundational paper defining contextual integrity (Washington Law Review 2004, seed)

## Entities

- [[agentcibench]] — CI disclosure benchmark for computer-use agents; 117 MCTS-generated scenarios, three failure modes

## Concepts

- [[contextual-integrity]] — privacy as appropriate information flow; the five flow parameters
- [[privacy-collapse]] — fine-tuning failure mode: more inappropriate disclosure, unchanged safety/capability; triggers, mechanism, mitigations
- [[privacy-leakage-in-agents]] — agents leak by their own judgment; three failure modes; fine-tuning can worsen it; cheap prompting helps
- [[prompt-defenses-for-contextual-privacy]] — system-prompt defenses: recipient-aware prompting cuts agent leakage and raises utility
- [[privacy-leakage-in-llm-memory]] — stub: can models use persistent memory appropriately?

## Synthesis

(populated as query answers are filed back)
