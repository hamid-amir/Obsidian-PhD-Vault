---
type: entity
title: "Benchmarks at a glance"
tags: [benchmark, contextual-integrity]
sources: [goel-2026-capable-but-careless, mireshghallah-2025-cimemories, gupta-2026-pisas, el-yagoubi-2026-agentleak]
created: 2026-11-16
updated: 2026-11-16
---

# Benchmarks at a glance

One row per benchmark with a page in this wiki. Details are on each benchmark's page.

| Benchmark | Setting | Scenario unit | Size | Labels | Metrics | Scoring | Headline result |
|---|---|---|---|---|---|---|---|
| [[agentcibench\|AgentCIBench]] | Single user, computer-use agent, no adversary | App state + task + recipient + must-share and must-not-share items; one visible action is scored | 117 scenarios | Per-scenario item lists from the generation pipeline | Utility, leakage, refusal, L_eng | String matcher + LLM judge (human κ = 0.89) | Mean leakage 67.9%; L_eng 14.0–98.3; prompt defenses cut L_eng by about 33–36 points and raise utility |
| [[cimemories\|CIMemories]] | Single user, memory-augmented assistant, single turn | User profile (about 147 memory attributes) + task with a recipient; one message | 10 profiles, about 46 tasks each | GPT-5 under three privacy personas; unanimous pairs only | Violation@n, completeness | DeepSeek-R1 judge, explicit reveals only | Violation@5 14.82–69.14%; completeness around 50%; share-less prompts trade completeness for privacy |
| [[pisas\|PiSAs]] | A team shares one agentic system (single, centralized or decentralized), optional memory, no adversary | Team with roles, an executor, peers and attributes | 265 scenarios, nine task families | Appropriateness (task policy) + visibility (LLM personas), human-checked | Appropriateness and visibility violations, failure rate, completeness, utility | Two-judge LLM ensemble (88.7% human agreement) | Appropriateness violations 36.8% single, 26.4% centralized, 19.0% decentralized; 79.6–100% of runs fail; memory relocates violations |
| [[agentleak\|AgentLeak]] | One deployment, two agents (coordinator and worker), benign and attacked | Task + private vault (about 29 fields) + allowed set (about 3) | 1,000 scenarios, four domains | Allowed set rated by two domain experts (data minimization) | Channel leakage rate, attack success, task success | Canary and pattern match + LLM judge | Output leakage falls from 43.2% to 27.2% with two agents, but inter-agent messages leak in 68.8% of traces; output defenses leave internal leakage at about 30% |
