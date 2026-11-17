1. cimemories paper claims that prompting defense didn't help with privacy leakage of memory. But [[goel-2026-capable-but-careless|agentcibench]] shows that these defenses can both lower the leakage and also improve utility. How so?
	1. My speculation is that the defense prompt in cimemories paper is not a defense at all! It just asks models to "share less". But defense prompts in agentcibench paper explicitly encourage model to consider the CI params like recipient. 

2. What about MCP access? When the model itself decide which tool to call and what context to get. (maybe similar to "Helpful" mode in privacy collapse paper???)

3. Test defense prompts on models after finetuning?

4. Create a ==multi-task (sequential tasks)== multi-user privacy leakage benchmark?! CIMemories has many tasks but one user, and PiSAs has many users but one task.

5. so agentleak showed that prompt based defenses (which are mainly output focused) decrease output leakage but have no effect on internal leakage between the agents. So maybe trying to ==find defenses that can actually decrease internal-channel leakage== would be a good thing to explore. Also trying to ==understand== why prompt based defenses has no effect on internal leakage?

6. If moving toward mitigation/defense, we can use the https://arxiv.org/pdf/2506.04245 paper where they introduced an RL framework that further enhances CI-aligned reasoning of LLMs, improving the ability of LLMs to reason about CI.
	1. does CI-RL survive later “helpful” fine-tuning, or does it collapse too?

7. If moving toward understanding, we can use the https://arxiv.org/abs/2506.15674 paper, where they showed LLMs have leaky thoughts during their reasoning traces. Explore why this happens.