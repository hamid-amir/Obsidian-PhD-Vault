
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
	
7. **Understanding direction: leaky thoughts.** [[green-2025-leaky-thoughts|Leaky Thoughts]] shows that LLMs leak private information in their reasoning traces. A starting point if we move toward understanding: why does this happen?
	1. Move toward the benchmarks that also consider model's internal thinking (not just its outputs)
	
8. **Multi-agent systems move leakage inside.** Going from one agent to several lowers leakage in the final output, but new leaks appear in inter-agent messages and shared memory, and prompt defenses fix only the output.
	- *Evidence:* in [[el-yagoubi-2026-agentleak|AgentLeak]], output leakage falls from 43.2% (one agent) to 27.2% (two), yet messages leak in 68.8% of traces and shared memory in 46.7%; 41.7% of traces are clean at the output and leak internally. A privacy prompt cuts output leakage from 48% to 19% but leaves internal leakage at 31% → 29%. [[gupta-2026-pisas|PiSAs]] shows a similar shift: per-user agents lower appropriateness violations (36.8% → 19.0%), and memory moves violations onto memory ("a relocation"). Whether total exposure rises is contested between the two papers: see [[internal-agent-flows-exposure-or-harm]].
	- *Claude's suggested direction: need-to-know at handoff.* AgentLeak's masking prototype cuts internal leakage from 31.5% to 2.4% at a 4.7-point task cost, but it is given the ground-truth allowed set, which a real system doesn't have. Build a handoff gate: the sending agent, or a small checker, infers from the subtask what the receiving agent needs and passes only that.
		- How close does an inferred allowed set get to the oracle one?
		- Can it keep completeness? Output filters cost completeness in PiSAs (97.0% → about 75–78%).
		- Is a recipient-aware prompt that names the worker as the recipient ("the worker needs only X") enough, or does it take a separate gate?
	- *First experiment:* on AgentLeak, compare no defense, the privacy prompt, a recipient-aware prompt, an inferred gate and the oracle mask, on internal leakage and task success.
	- Overlaps with question 5; the focus here is the gap between the oracle filter and anything deployable. Related: the "context lost at handoff" idea in [[multi-agent scenario]] (candidate idea 3). Novelty not checked; closest prior work in that note's literature record is "Judging Before Propagation" (not yet read).

## Ideas from the brainstorm with Claude (2026-11-17)

Claude's suggestions; novelty not yet checked.

9. **Does reasoning distillation cause privacy collapse?** Reasoning models are often made by fine-tuning on a teacher's reasoning traces, and [[green-2025-leaky-thoughts|Leaky Thoughts]] finds such traces restate private data; the Qwen2.5-32B-based reasoning models there leaked more than plain Qwen (first-pass reading). Is distillation a form of benign fine-tuning that triggers [[privacy-collapse]]? Compare base vs distilled models, then distill with private data swapped out of the traces. Design note: [[reasoning-distillation-privacy-collapse]].
10. **Agentic RL as the "helpful" reward.** [[goel-2026-privacy-collapse]] tested SFT only. Agent RL that rewards task completion may reward information use without asking. Does CI drop across RL checkpoints, and does a CI reward term ([[lan-2025-ci-reasoning-rl]]) prevent it? Training-time version of question 2.
11. **Leakage the judges can't see.** Judges and keyword rewards check whether a protected attribute appears. Agents can disclose inferences or paraphrases instead ("she may need a lighter workload"). Build a test set of inferential and paraphrased leaks and measure how much current judges miss.
12. **Cheap repair of a collapsed model.** [[goel-2026-privacy-collapse]] finds late-layer privacy directions flip after fine-tuning while commonsense ones stay aligned. Can adding the base model's privacy direction back, or editing only those layers, restore privacy without retraining? See [[privacy-collapse]].

## Open questions from the wiki


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

