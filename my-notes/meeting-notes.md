
Nov 18, 2026


- ~~agentic setups collousin between agents (cowork with other agents without human notice, hidden communication, helps them achive their goal. communications can happen using images, texts, (signals, watermark)~~
- ~~matryoshka representation: reduce dimension.~~
- ~~multi agent communicatio chaneel is covert. if access to shared embedding access and human can see, show same retrieval accuracy~~
- ~~general rag, agents can communicate throgh embedding dimensions~~
- ~~its an attack idea because of agents can use matrysjka  embedding dims to communicate~~


- ~~defense idea: John morris papers~~
- ~~in a matry setup,~~ 
----

Nov 27, 2026

- **Chain of thought monitorability:** train smaller models/agents that look into the reasoning traces of frontier models to detect intention for misbehave.
	- frontier models don't expose weights/logits, so classic interp doesn't apply; small model acts as a "mirror" on the CoT.

- **Human-agents collaboration:** develop methods/benchmarks/measures for the people's offloading their thinking to the agents in long-horizon tasks.
	- offloading paper: measure how much of the cognitive thinking, a human has given to the agent.
	- use interpretability methods to investigate signals in the model that incentivize the offloading, or how to disincentivize it
	- for example design a RL pipeline or sft training method that decrease this offloading score of the model (increases human agency)
	- offloading paper = [Offloading Score: Measuring AI Reliance Through Counterfactual Workflows](https://arxiv.org/abs/2605.29392) (Diyi Yang's lab, Stanford; NeurIPS 2026 oral; [project page](https://vishakhpk.github.io/measuring-reliance/)). estimates a counterfactual human-only workflow, offloading = fraction of steps saved by the tool. execution most offloaded, but feedback is too.
	- [SWE-Together: Evaluating Coding Agents in Interactive User Sessions](https://arxiv.org/abs/2606.29957) (Meta; [togetherbench.com](https://togetherbench.com), [code](https://github.com/Togetherbench/SWE-Together)): real human–coding-agent sessions turned into Docker tasks. Anmol's master student uses it to find the "pivotal action" (intervene on each step, roll out counterfactual, measure test/task reward).
	- agent memory / compaction as a causal variable of offloading → e.g. RL agents to save memory so the human isn't cut out.
	- multi-agent version: how does human agency change when working with a swarm of agents (losing track of subagents)?
	- general-purpose methods + benchmarks first, domain-specific later. Irina is interested in this.
