> [!warning] Written by Claude on 2026-11-26, at Hamid's request
> Compiled from a literature search (abstracts, search summaries and a few HTML pages), not from full reads. Treat every number as provisional until the paper is ingested into the wiki. None of these papers has a wiki page yet. Slides: `Claude outputs/thesis_proposal_information_flow.pptx`.

# Thesis idea: information flow across boundaries in LLM agent systems

**The question:** what information should cross a boundary in an agent system, in what form, and at what cost?

The same question shows up at three boundaries:

| Boundary | Direction | What crosses | What goes wrong |
|---|---|---|---|
| **Memory** (time) | past self → future self | facts, experiences, skills, or weight updates | stale memories are kept; useful ones are never written |
| **Latent communication** (models) | agent A's internals → agent B | hidden states or KV caches instead of text | rich but opaque; unclear what the receiver actually uses |
| **Delegation** (roles) | orchestrator ⇄ worker | briefs going down, reports coming up | evidence is dropped in the report; the lead never knows |

The trade-off is the same everywhere: compress and you lose information; send everything and you pay a "protocol tax" (attention, latency, tokens).

**Where the privacy work fits:** [[contextual-integrity|Contextual integrity]] is already a theory of *appropriate* information flow. In this framing the privacy work becomes the thread about flows that *should not* happen at each boundary, e.g. [[mireshghallah-2025-cimemories|CIMemories]] (memory), [[el-yagoubi-2026-agentleak|AgentLeak]] and [[gupta-2026-pisas|PiSAs]] (delegation and internal channels). See also [[multi-agent literature]] and [[research-agenda]].

## Running example (illustrative)

A research-assistant team answers *"Which cloud provider should my startup use?"*: a lead agent, three workers (pricing, compliance, performance) and a memory store.

1. **Memory:** in March the user said "EU data only". In June they mention moving HQ to Texas. Does the agent notice the stored fact is stale?
2. **Latent communication:** workers could hand the lead their hidden states instead of a text summary. Faster and richer, but what is actually in them?
3. **Delegation:** the pricing worker reads $0.12/h on one page and $0.15/h on another and reports "$0.12/h". The conflict disappears before the lead can weigh it.

## Why now

- **Memory:** naive retrieval over the raw history ranks first in 6 of 7 benchmark–model settings against dedicated agent-memory systems ([Raw-RAG, arXiv 2610.30755](https://arxiv.org/abs/2610.30755)).
- **Latent:** latent collaboration gives ~4× faster inference with 70–84% fewer output tokens ([LatentMAS, ICML 2026](https://arxiv.org/abs/2511.20639)).
- **Delegation:** multi-agent variants degrade sequential reasoning tasks by 39–70% compared with a single agent ([Kim et al., arXiv 2512.08296](https://arxiv.org/abs/2512.08296)).

---

## 1. Memory (across time)

**Core question:** should an agent write abstractions, raw logs or weight updates, and when?

### What we know

- **External memory does not escape forgetting.** Old and new experiences compete at retrieval time; the stability–plasticity trade-off moves from weights to memory access. Abstract procedural memories transfer better than raw trajectories, and negative transfer hurts the hardest cases most. ([Hu, Long & Wang, arXiv 2604.27003](https://arxiv.org/abs/2604.27003))
- **Memory as learning.** Memory-Centric Continual Learning argues memory should be judged by whether it improves future actions, not by what it retrieves, and calls it open whether memory can drive real continual learning. ([MCCL, arXiv 2610.29357](https://arxiv.org/abs/2610.29357)) Evaluation framework for continual learning in agents: [AgentCL, arXiv 2606.02461](https://arxiv.org/abs/2606.02461).
- **Memory operations can be learned.** Store, retrieve, update, summarize and discard trained as RL actions ([AgeMem, arXiv 2601.01885](https://arxiv.org/abs/2601.01885)); memory access as an MDP that decides when to retrieve, consolidate or forget ([MemCon, arXiv 2607.13591](https://arxiv.org/abs/2607.13591)). [AdaMem (arXiv 2606.21144)](https://arxiv.org/abs/2606.21144) personalizes the write policy and reports that executing the policy is "a central limitation".
- **Moving memory into weights.** A position paper claims retrieval-based memory has a provably lower ceiling than weight-based memory on novel tasks ([arXiv 2604.27707](https://arxiv.org/abs/2604.27707)). Early consolidation methods: [Sleep (arXiv 2606.03979)](https://arxiv.org/abs/2606.03979), [Memory Depth (arXiv 2606.26806)](https://arxiv.org/abs/2606.26806).

### Gaps

1. **Does writing memory even matter?** Raw-history retrieval matches or beats explicit memory systems: Raw-RAG ranks first in 6 of 7 settings ([arXiv 2610.30755](https://arxiv.org/abs/2610.30755)); six explicit memory systems show no consistent advantage over raw-history RAG ([Zhou et al., arXiv 2610.24671](https://arxiv.org/abs/2610.24671)); on LoCoMo the retrieval method moves accuracy ~20 points but the write strategy only 3–8 ([Yuan et al., arXiv 2603.02473](https://arxiv.org/abs/2603.02473)). ==All of this is conversational fact recall. Nobody has mapped when abstraction pays off (experience and skill memory, task streams, tight budgets).==
2. **Noticing stale memories.** Agents struggle when new information implicitly overrides an old memory, and an external memory module does not fix it ([STALE, arXiv 2605.06527](https://arxiv.org/abs/2605.06527)).
3. **When to consolidate into weights.** No account of what to consolidate, when, or how it interferes with what is already there.
4. **Memory shared across agents.** A position paper calls multi-agent memory consistency the most pressing open challenge ([arXiv 2603.10062](https://arxiv.org/abs/2603.10062)); [COLLABMEM (EMNLP 2026)](https://aclanthology.org/2026.emnlp-main.1194/) finds reliable collaborative memory still needs explicit visibility, provenance, authority and retention policies.

> [!question] RQ1
> When does constructing memory beat keeping raw logs, and can a learned write-and-consolidate policy exploit it?

---

## 2. Latent communication (across models)

**Core question:** what is actually transmitted when agents exchange hidden states or KV caches instead of text, and when is it better?

Text is a lossy channel: each token carries at most log₂(vocabulary) ≈ 15 bits, while a hidden state carries far more ([survey, arXiv 2606.05711](https://arxiv.org/abs/2606.05711)).

### What we know

- **Cache-to-Cache** (ICLR 2026): projects and fuses one model's KV cache into another's; ~3–5 points over text exchange, ~2× lower latency. ([arXiv 2510.03215](https://arxiv.org/abs/2510.03215))
- **LatentMAS** (ICML 2026): training-free latent thoughts plus shared latent working memory; up to +14.6 points, 70–84% fewer output tokens, ~4× faster. ([arXiv 2511.20639](https://arxiv.org/abs/2511.20639))
- **Interlat** (ACL 2026): trained latent messages with learned compression; tested on ALFWorld and MATH, reported to work across models. ([arXiv 2511.09149](https://arxiv.org/abs/2511.09149))
- **Vision Wormhole** (2026): different model families talk through the vision input of VLMs. ([arXiv 2602.15382](https://arxiv.org/abs/2602.15382))
- **Survey** with a WHAT / WHICH / HOW taxonomy and open problems: cross-architecture alignment, security, compression, theory, evaluation, latent CoT vs communication, deployment. ([Liu, arXiv 2606.05711](https://arxiv.org/abs/2606.05711))

### Gaps

1. **Is the content used?** A causal audit of LatentMAS separates "receiving any message" from "this message's content". On MATH-500 with Qwen3-8B, +8.1 of the +10 point gain also appears with a message from a different example; the content-attributable part is +1.9 with an interval crossing zero. Directions flip between 4B and 8B. Samples are small (40–100). ([Zhang & Emu, arXiv 2607.26773](https://arxiv.org/abs/2607.26773))
2. **What is encoded?** Task identity and agent roles are linearly decodable, most of the representation space is unused, and agents are highly redundant; the richness seems to reflect base-model geometry, not learned coordination (ICML 2026 workshop, [Lu & Deshpande](https://icml.cc/virtual/2026/69328)).
3. **Beyond math and QA.** Tool use and long-horizon agentic tasks are untested; Interlat deliberately excludes tool use, retrieval and debate.
4. **Theory and protocols.** Most methods assume the same model on both ends; no estimate of usable channel capacity; no hybrid text + latent protocol. Latent messages also can't be inspected: the hidden state can be altered while the visible message looks fine ([KV-cache integrity, arXiv 2606.28958](https://arxiv.org/abs/2606.28958)).

> [!question] RQ2
> What do latent channels carry, what does the receiver actually use, and when do they beat text on agentic tasks?

*Fit:* the probing and steering background from [[activation-probing-and-steering]] carries over directly; runs on open 4–8B models.

---

## 3. Delegation (across roles)

**Core question:** what should cross between an orchestrator and its workers, in both directions?

### What we know

- **Failure taxonomy:** 14 failure modes in 3 categories (system design, inter-agent misalignment, task verification) across 7 frameworks ([MAST, NeurIPS 2025, arXiv 2503.13657](https://arxiv.org/abs/2503.13657)).
- **Scaling science:** 180 configurations; multi-agent variants degrade sequential reasoning by 39–70%; coordination has diminishing returns as the single-agent baseline gets stronger; tool-heavy tasks pay extra overhead ([Kim et al., arXiv 2512.08296](https://arxiv.org/abs/2512.08296)).
- **Information-bottleneck view:** multi-agent gains appear when context reduction outweighs relay information loss, and shrink for stronger models ([When Do Multi-Agent Systems Help?, NeurIPS 2026](https://neurips.cc/virtual/2026/poster/153823)).
- **Protocol tax:** a message's value is the executor's gain minus its protocol tax; compressed messages can beat raw context ([Bayes-Sufficient Compression Is Not Enough, NeurIPS 2026](https://neurips.cc/virtual/2026/poster/155601)).
- **What to say:** effective messages preserve action-centred information; no fixed strategy wins everywhere ([What Should Agents Say?, arXiv 2606.05304](https://arxiv.org/abs/2606.05304)).
- **Scaling to many agents:** a shared datastore with bounded search fixes reading burden and fragility, scaling to 128 agents ([ScaleMAS, arXiv 2610.16120](https://arxiv.org/abs/2610.16120)).
- **Losses at the boundary:** delegation can degrade search when the needed knowledge is unavailable to the orchestrator; retrieval failures get lost across the boundary ([Lost in Delegation, arXiv 2611.16526](https://arxiv.org/abs/2611.16526)). In 73 controlled conflicts, the largest evidence loss is at the worker-to-lead handoff ([Multi-Agent Memory Inconsistency, arXiv 2611.22245](https://arxiv.org/abs/2611.22245)).

### Gaps

1. **The upward channel.** Workers' reports lose the most evidence, yet almost all work optimizes the brief going down.
2. **When to stop.** A survey of RL for multi-agent systems found no RL method for the stopping decision as of May 2026 ([arXiv 2605.02801](https://arxiv.org/abs/2605.02801)). Orchestrator RL is costly: MAS-Orchestra used over 1B tokens for 100 update steps ([Orch-RM, arXiv 2606.13598](https://arxiv.org/abs/2606.13598)).
3. **Receiver-blind messages.** Senders do not model what the receiver already knows (a theory-of-mind gap).
4. **Compounding over many hops.** How losses accumulate across long, multi-hop tasks is untested.

> [!caution] Crowding
> Single-handoff theory is already in two NeurIPS 2026 papers, and "What Should Agents Say?" studies message content. This overlaps with my handoff-gate idea (Idea A) and the "is need predictable?" idea (Idea B) in [[research-agenda]] question 8. The upward channel and multi-hop effects are still open.

> [!question] RQ3
> What do workers drop when they report back, and how should reports and stop decisions use what the lead already knows?

---

## Gap map

| Boundary | Established | Main open gaps | Crowding (Claude's estimate) | Cost to start |
|---|---|---|---|---|
| Memory | learned memory operations; recall benchmarks | when writing beats raw logs; staleness; consolidation into weights | medium | low: API + small open models |
| Latent communication | gains on math and QA; 2–4× speedups | is the content used? agentic tasks; usable capacity | low–medium | medium: open 4–8B models, GPUs |
| Delegation | failure taxonomy; single-handoff theory | upward-channel loss; stop decision; multi-hop compounding | medium–high (theory) | low: API models, no training |

## Proposed roadmap (to discuss)

1. **Year 1 · Delegation:** measure what workers drop when they report back; test reports that account for what the lead already knows. Cheapest start.
2. **Year 2 · Latent communication:** causal audits and probing of latent channels on agentic tasks; design a hybrid text + latent protocol.
3. **Year 3 · Memory:** map when memory construction beats raw logs; learned write-and-consolidate policy; memory shared across agents.

Shared methods: controlled ablations, causal interventions, probing, information-theoretic measures.

## Open questions for the supervisor meeting

1. One thesis or three papers?
2. Start with delegation (cheapest) or latent communication (least crowded)?
3. What GPU budget is realistic?
4. Keep contextual integrity as the running thread, or as a separate chapter?
5. For each boundary: benchmark, method, or measurement science?
