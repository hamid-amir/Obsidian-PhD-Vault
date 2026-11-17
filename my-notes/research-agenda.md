# Research agenda

My open questions, ideas and topics for the meetings. The research questions keep their numbers because wiki pages refer to them by number. All open questions from the wiki live here too: lines marked *Wiki:* add what the wiki knows, and the last section collects the wiki's own questions.

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
	- *Wiki:* untested in either paper whether recipient-aware prompts break the violation–completeness trade-off on CIMemories. CIMemories tried only minimisation-style prompts, and AgentCIBench scopes long-term memory out. See [[prompt-defenses-for-contextual-privacy#Free lunch or trade-off]] and [[privacy-leakage-in-llm-memory]].
	
2. **Tool and context choice (MCP access).** When the model itself decides which tool to call and what context to fetch, is it in something like the "helpful" mode of the [[goel-2026-privacy-collapse|privacy collapse]] paper?
	- *Wiki:* not tested anywhere yet. See [[privacy-collapse]] (what triggers it) and [[privacy-leakage-in-agents]].

3. **Defenses after fine-tuning.** Do defense prompts still work on models whose privacy collapsed after fine-tuning?
	- *Wiki:* [[goel-2026-privacy-collapse]] did not test this. See [[prompt-defenses-for-contextual-privacy]] and [[privacy-collapse]].

4. **A multi-user, multi-task benchmark.** Build a privacy leakage benchmark with ==sequential tasks and several users==. CIMemories has many tasks but one user; [[gupta-2026-pisas|PiSAs]] has many users but one task.

5. **Internal-channel leakage.** [[el-yagoubi-2026-agentleak|AgentLeak]] shows that prompt defenses, which mainly target the output, reduce output leakage but barely change leakage between agents.
	- ==Find defenses that actually reduce internal-channel leakage.==
	- ==Understand why== prompt defenses don't affect it.
	- *Wiki:* AgentLeak did not test recipient-aware prompts, so whether any prompt can reach internal channels is still open. See [[prompt-defenses-for-contextual-privacy]] and [[agentleak]].
	
6. **Mitigation direction: CI-RL.** [[lan-2025-ci-reasoning-rl]] uses RL to strengthen a model's reasoning about CI. A starting point if we move toward defenses.
	- Does CI-RL survive later "helpful" fine-tuning, or does it collapse too? See [[privacy-collapse]].
	- *Wiki:* does training CI reasoning in, rather than prompting for it, change the prompt-defense picture? (Paper is a seed, not yet read.)
	
7. **Understanding direction: leaky thoughts.** [arXiv 2506.15674](https://arxiv.org/abs/2506.15674) shows that LLMs leak private information in their reasoning traces. A starting point if we move toward understanding: why does this happen?

## Open questions from the wiki

Moved here from the wiki pages on 2026-11-17. These come from the papers' gaps or from discussions with Claude, not from me.

**Prompt defenses** ([[prompt-defenses-for-contextual-privacy]])
- Do the gains survive end-to-end UI execution? [[goel-2026-capable-but-careless|AgentCIBench]] tested defenses only in the state-grounded setting.

**Privacy collapse** ([[privacy-collapse]])
- Is ungated collapse a separable "mode", or diffuse damage? The backdoor experiment in [[goel-2026-privacy-collapse]] (§4.5) shows the behaviour can be gated, which does not by itself settle this.

**Memory** ([[privacy-leakage-in-llm-memory]])
- In [[mireshghallah-2025-cimemories|CIMemories]], violations grow with more samples and more memories. How much of this "accumulation" is model behaviour and how much comes from the worst-case metric (worst over tasks and samples)?

**Internal agent flows** ([[internal-agent-flows-exposure-or-harm]])
- When is an agent a recipient in the [[contextual-integrity|CI]] sense? Claude's candidate answer: it depends on who the agent's principal is and where its context can flow next. Neither [[agentleak|AgentLeak]] nor [[pisas|PiSAs]] defines this.
- How often does internal exposure become disclosure to a person? AgentLeak measures the first of its three levels (policy violation) and "the conditions" for the second; nothing in the wiki measures how often that turns into an actual breach.
- Why do coordinators over-share? AgentLeak's leak rates barely change across threat levels, which suggests a delegation habit (passing on the whole context). Untested inference.

