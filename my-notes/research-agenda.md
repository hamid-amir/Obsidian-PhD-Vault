# Research agenda

My open questions, ideas and topics for the meetings. The research questions keep their numbers because wiki pages refer to them by number.

## For the next meeting

1. **Timeline:** is there a schedule for the project, for example one month of literature review first?
2. **Focus:** what is our main contribution: another benchmark or dataset, explaining why privacy leakage happens, or mitigating/defense it in a specific scenario?
3. **Reading:** should we read papers and explain them to each other?

Topics to discuss:

- **Memory:** how do models decide what to save, and how can we make sure only appropriate information is saved?
- **Multi-agent communication:** what are the failure modes? See [[multi-agent scenario#Failure modes]].

## Research questions and ideas

1. **Why do prompt defenses fail for memory but work for agents?** [[mireshghallah-2025-cimemories|CIMemories]] finds that prompting doesn't fix privacy leakage from memory, while [[goel-2026-capable-but-careless|AgentCIBench]] finds that defense prompts both lower leakage and improve utility.
	- My speculation: the CIMemories prompt is not really a defense. It only asks the model to "share less", while the AgentCIBench prompts explicitly make the model consider CI parameters such as the recipient.
	
2. **Tool and context choice (MCP access).** When the model itself decides which tool to call and what context to fetch, is it in something like the "helpful" mode of the [[goel-2026-privacy-collapse|privacy collapse]] paper?

3. **Defenses after fine-tuning.** Do defense prompts still work on models whose privacy collapsed after fine-tuning?

4. **A multi-user, multi-task benchmark.** Build a privacy leakage benchmark with ==sequential tasks and several users==. CIMemories has many tasks but one user; [[gupta-2026-pisas|PiSAs]] has many users but one task.

5. **Internal-channel leakage.** [[el-yagoubi-2026-agentleak|AgentLeak]] shows that prompt defenses, which mainly target the output, reduce output leakage but barely change leakage between agents.
	- ==Find defenses that actually reduce internal-channel leakage.==
	- ==Understand why== prompt defenses don't affect it.
	
6. **Mitigation direction: CI-RL.** [[lan-2025-ci-reasoning-rl]] uses RL to strengthen a model's reasoning about CI. A starting point if we move toward defenses.
	- Does CI-RL survive later "helpful" fine-tuning, or does it collapse too?
	
7. **Understanding direction: leaky thoughts.** [arXiv 2506.15674](https://arxiv.org/abs/2506.15674) shows that LLMs leak private information in their reasoning traces. A starting point if we move toward understanding: why does this happen?
