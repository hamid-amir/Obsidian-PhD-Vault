# Wiki Index

The catalog of all pages in this wiki. Each entry: a wikilink to the page and a one-line summary. The LLM reads this first when answering queries to identify candidate pages.

Keep summaries tight — one line each. The index is engineered to be cheap to read; a fat index defeats its purpose.

When this file exceeds ~300 lines or the wiki passes ~150 pages, shard into `wiki/indexes/<type>.md` and replace this file with a directory of shards. See the `scaling-playbook.md` reference in the `llm-wiki` skill for the migration procedure.

---

## Sources

- [[el-yagoubi-2026-agentleak]] — AgentLeak: two-agent systems leak less in the output (27.2%) but over-share in inter-agent messages (68.8%) and memory, attacker or not; output defenses miss it; minimization, not CI (arXiv 2026, v3)
- [[goel-2026-capable-but-careless]] — AgentCIBench: 15 computer-use agents leak without an attacker (mean 67.9%); capability ≠ carefulness; prompt defenses cut leakage ~35 pts (EMNLP 2026)
- [[goel-2026-privacy-collapse]] — benign fine-tuning (helpful, empathetic, support data) breaks contextual privacy while safety/capability hold; can be backdoored; data filtering helps partly (ACL 2026)
- [[gupta-2026-pisas]] — PiSAs: teams sharing one agent system spill info between teammates; per-user agents and memory relocate leaks; 79.6–100% of runs fail (arXiv 2026, v2)
- [[mireshghallah-2025-cimemories]] — CIMemories: memory-augmented LLMs reveal withheld attributes (Violation@5 15–69%); share-less prompts trade completeness for privacy (arXiv 2025)
- [[nissenbaum-2004-privacy-as-contextual-integrity]] — foundational paper defining contextual integrity (Washington Law Review 2004, seed)

## Entities

- [[benchmarks-at-a-glance]] — summary table of all benchmarks with a page (setting, unit, size, labels, metrics, scoring, headline, caveats); add a row on every benchmark ingest
- [[pisas]] — CI benchmark for multi-user agentic systems; 265 scenarios, nine task families, appropriateness + visibility labels, three topologies
- [[cimemories]] — CI benchmark for persistent memory; 10 profiles, ~147 attributes, GPT-5 persona labels; Violation@n and completeness
- [[agentleak]] — benchmark for internal-channel leakage in multi-agent systems; 1,000 scenarios, vault vs allowed set, seven channels, two-agent coordinator-worker
- [[agentcibench]] — CI disclosure benchmark for computer-use agents; 117 MCTS-generated scenarios, three failure modes

## Concepts

- [[contextual-integrity]] — privacy as appropriate information flow; the five flow parameters
- [[privacy-collapse]] — fine-tuning failure mode: more inappropriate disclosure, unchanged safety/capability; triggers, mechanism, mitigations
- [[privacy-leakage-in-agents]] — agents leak by their own judgment; settings map (single-user, multi-user, multi-agent); failure modes; internal channels; fine-tuning can worsen it
- [[prompt-defenses-for-contextual-privacy]] — system-prompt defenses: help agents at no utility cost (AgentCIBench) but trade completeness for privacy in memory (CIMemories); contested
- [[privacy-leakage-in-llm-memory]] — models over-share stored memories; violations grow with samples and memory size; fine-tuning worsens it

## Synthesis

- [[internal-agent-flows-exposure-or-harm]] — is an agent-to-agent flow a privacy violation? AgentLeak vs PiSAs; when internal exposure matters; open question on agents as CI recipients
