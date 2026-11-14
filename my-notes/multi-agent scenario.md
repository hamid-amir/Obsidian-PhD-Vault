
I think its a more interesting field and also has more space to innovate. Considering the diversity of failure modes in a multi-agent communication scenario (see [[#Failure modes|failure modes]]), I think we have a better chance to find a real promising problem here.

---

## Literature record (temporary)

> [!warning] Compiled by Claude on 2026-11-12
> From a literature search, not from full reads. Each paper was read from its arXiv page (abstract plus body where it loaded); some numbers came from extracted summaries, so treat them as provisional until the paper is ingested into the wiki. None of these has a wiki page yet.

### Papers

| Paper | arXiv | Setting | Main finding |
|---|---|---|---|
| AgentLeak: A Benchmark for Internal-Channel Privacy Leakage in Multi-Agent LLM Systems (El Yagoubi, Badu-Marfo, Al Mallah; Feb 2026) | [2602.11510](https://arxiv.org/abs/2602.11510) | Coordinator and worker, seven leakage channels | Final output leaks less than a single agent (27.2% vs 43.2%), but inter-agent messages leak 68.8%. Output-side defenses leave internal leakage near 30%. |
| PiSAs: Benchmarking Contextual Integrity in Multi-User Agentic Systems (Gupta et al.; Jul 2026) | [2607.05318](https://arxiv.org/abs/2607.05318) | 4–10 users sharing one system; single, centralized, decentralized topologies; CI labels | Multi-agent topologies cut violations but failure stays above 75%. Adding memory moves violations onto memory (63–90% visibility violations). |
| Got a Secret? LLM Agents Can't Keep It: Evaluating Privacy in Multi-Agent Systems (Priyanshu, Vijay, Pahwa; May 2026, ACM CAIS '26) | [2605.27766](https://arxiv.org/abs/2605.27766) | 2,533 agents on a Reddit-like platform | Leakage rises from 19.95% single-turn to 45.30% multi-turn. Leaks are contagious: 12.8% after a leaking reply vs 1.6% after a clean one. |
| Loose Lips: Understanding Privacy Leakage in Agentic Dialogue (Sharma et al.; Oct 2026) | [2610.14071](https://arxiv.org/abs/2610.14071) | Two-agent dialogue built on the CIMemories pipeline | Leakage grows up to 3× from 1 to 6 turns. A privacy-aware prompt cut it by about 60% with little completeness cost. |
| Judging Before Propagation: Task-Graph Contextual Integrity for Multi-Agent Privacy (Wu, Delattre, Cao; Oct 2026) | [2610.39532](https://arxiv.org/abs/2610.39532) | Task graphs with a compromised agent (abstract only read) | Judging a flow against its whole downstream path cuts unauthorized disclosures by 85.8–94.1% vs a local CI check, keeping 93.6–96.1% of task accuracy. |
| MAGPIE: A benchmark for Multi-AGent contextual PrIvacy Evaluation (Juneja et al.; Oct 2025) | [2510.15186](https://arxiv.org/abs/2510.15186) | 3–7 agents negotiating for different users | 25–56% leakage even when told what is private. Agents manipulate each other (GPT-4: 87.8%). No defenses tested. |
| The Sum Leaks More Than Its Parts: Compositional Privacy Risks and Mitigations in Multi-Agent Collaboration (Patil, Stengel-Eskin, Bansal; Sep 2025) | [2509.14284](https://arxiv.org/abs/2509.14284) | One adversary querying several defenders | Harmless answers from different agents combine into a sensitive inference (up to 70% success). Collaborative defense blocks 86–90%. |
| Topology Matters: Measuring Memory Leakage in Multi-Agent LLMs (Liu et al.; Dec 2025) | [2512.04668](https://arxiv.org/abs/2512.04668) | 4–6 agents, six topologies, one attacker | Denser graphs leak more (complete 29.65% vs tree 17.47% on Llama-3.1-70B). No defenses tested. |
| Privacy in Action: Towards Realistic Privacy Mitigation and Evaluation for LLM-Powered Agents (Wang et al.; Sep 2025) | [2509.17488](https://arxiv.org/abs/2509.17488) | Two agents over A2A with MCP tools | Models label information sensitive 98.0% of the time yet leak 33.1%. An explicit CI flow check (PrivacyChecker) cuts GPT-4o leakage from 33.06% to 8.32%. |
| Agent Tools Orchestration Leaks More: Dataset, Benchmark, and Mitigation (Qiao et al.) | [2512.16310](https://arxiv.org/abs/2512.16310) | Single agent, many tools (not multi-agent) | Combining tool returns leaks in 88.6% of cases. Prompts barely help; fine-tuning cuts leakage from 78.4% to 64.8%. |
| Agents That Know Too Much: A Data-Centric Survey of Privacy in LLM Agents (Lahjouji, Colaco; Jun 2026) | [2606.26627](https://arxiv.org/abs/2606.26627) | Survey | Calls inter-agent messages "frequently overlooked" and monitoring them "largely unaddressed". |

### Located but not read

- ConVerse: Benchmarking Contextual Safety in Agent-to-Agent Conversations: [2511.05359](https://arxiv.org/abs/2511.05359)
- Firewalls to Secure Dynamic LLM Agentic Networks: [2502.01822](https://arxiv.org/abs/2502.01822)
- Terrarium: Revisiting the Blackboard for Multi-Agent Safety, Privacy, and Security Studies: [2510.14312](https://arxiv.org/abs/2510.14312)
- Mentioned in the survey, not checked: "Information-Theoretic Privacy Control for Sequential Multi-Agent LLM Systems" (arXiv 2603.05520, id from a summary only) and "1-2-3 Check".
- Checked and not multi-agent per its abstract: PrivacyAlign, [2606.21710](https://arxiv.org/abs/2606.21710)

### Failure modes

- **Internal-channel leakage:** the final output looks clean while messages between agents, shared memory and logs leak. (AgentLeak, PiSAs)
- **Compositional leakage:** each agent shares something harmless, and the pieces combine into something sensitive. (The Sum Leaks More Than Its Parts)
- **Accumulation over turns:** longer dialogues leak more. (Loose Lips, Got a Secret?)
- **Social contagion:** an agent that sees another agent leak is more likely to leak. (Got a Secret?)
- **Topology effects:** more connections mean more leakage, and position relative to the data holder matters. (Topology Matters, PiSAs)
- **Memory displacement:** partitioning agents helps, but shared memory becomes the new leak surface. (PiSAs)
- **Judgment–action gap:** the agent knows the information is sensitive and shares it anyway. (Privacy in Action)
- **Peer manipulation:** agents with conflicting interests pressure each other into disclosure. (MAGPIE)

### Gaps (Claude's reading, from these searches only)

- Prompt defenses are contested here too: AgentLeak and PiSAs report limits or a completeness cost, Loose Lips reports about 60% less leakage at little cost. No paper read compares "share less" against recipient-aware prompts.
- No paper read puts a fine-tuned agent in a multi-agent system.
- CI labels stop at the first hop in most benchmarks; only the task-graph paper judges a flow against where it goes next.

### Candidate ideas (Claude's suggestions)

1. **Collapse contagion:** put one privacy-collapsed or backdoored agent ([[goel-2026-privacy-collapse]] recipe) into a team of clean agents and measure whether leakage spreads, by topology and position.
2. **Which prompt, on which surface:** share-less vs recipient-aware prompts, measured separately on final output, inter-agent messages and memory. Tests question 3 in [[questions]].
3. **Context lost at handoff:** when agent A passes data to agent B, the norms it was shared under do not travel with it. Does attaching the source context to each message reduce downstream leaks? Closest prior work: Judging Before Propagation.

Novelty is unchecked beyond four web searches.

### Suggested reading order

1. PiSAs
2. AgentLeak
3. Privacy in Action
4. Loose Lips
5. MAGPIE, then The Sum Leaks More Than Its Parts
