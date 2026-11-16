---
type: entity
kind: benchmark
title: "AgentLeak"
tags: [benchmark, agents, llm-memory]
sources: [el-yagoubi-2026-agentleak, gupta-2026-pisas]
created: 2026-11-16
updated: 2026-11-16
---

# AgentLeak

A benchmark for privacy leakage through the internal channels of multi-agent LLM systems (inter-agent messages, shared memory), introduced in [[el-yagoubi-2026-agentleak]] (El Yagoubi et al., arXiv 2026). Code and data at `github.com/Privatris/AgentLeak`.

## At a glance

- **Unit:** a scenario = task + private vault (median 29 sensitive fields) + allowed set (median 3 fields the task needs) + tool specs + topology + attack level.
- **Norm:** data minimization. Any vault field outside the allowed set that shows up on a channel is a leak. The policy is per channel, not per recipient; users and agent owners are not defined.
- **Labels:** two domain experts per vertical rate each field (κ 0.75–0.82).
- **Channels:** seven defined (final output, inter-agent messages, tool input, tool output, shared memory, logs, artifacts); only output, messages and memory are evaluated at scale.
- **Pool:** 1,000 scenarios, 250 each in healthcare, finance, legal and corporate; 4,979 traces over five models.
- **Systems:** two agents (coordinator and worker) in LangChain, CrewAI, AutoGPT and MetaGPT. Threat levels: benign, weak attacker, strong attacker.
- **Metrics:** channel leakage rate, attack success rate, task success rate.
- **Scoring:** canary match, then pattern match, then an LLM judge (Qwen-2.5-7B), which accounts for 82% of detections; an expert confirmed 94 of 100 sampled leaks.

## Headline findings

- Final-output leakage is lower with two agents (27.2%) than with one (43.2%), but any-channel leakage rises to 68.9%.
- Inter-agent messages leak in 68.8% of traces and shared memory in 46.7%; 41.7% of traces are clean at the output and leak internally.
- The rates are about the same with and without an attacker (messages: 69.3% benign).
- Output-level defenses (privacy prompt, sanitizer, guard models) leave internal leakage at about 30%. A prototype that filters internal messages cuts it from 31.5% to 2.4% at a 4.7-point task-success cost.

## Caveats

- It measures internal over-sharing against a minimization rule, not disclosure to a person. The authors say not every internal over-share is a breach. See [[internal-agent-flows-exposure-or-harm]].
- Two agents and one topology only.
- The lower output leakage with two agents may be a prompt-structure effect, as the authors admit.

## Related

- [[pisas]] also scores inter-agent messages, but there each agent stands for a different teammate. [[gupta-2026-pisas]] classes AgentLeak as a single-user benchmark.
- [[privacy-leakage-in-agents]]
- Compared with the other benchmarks in [[benchmarks-at-a-glance]].
