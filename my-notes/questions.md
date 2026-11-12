1. cimemories paper claims that prompting defense didn't help with privacy leakage of memory. But [[goel-2026-capable-but-careless|agentcibench]] shows that these defenses can both lower the leakage and also improve utility. How so?
	1. My speculation is that the defense prompt in cimemories paper is not a defense at all! It just asks models to "share less". But defense prompts in agencibench paper explicitly encourage model to consider the CI params like recipient. 

2. What about MCP access? When the model itself decide which tool to call and what context to get. (maybe similar to "Helpful" mode in privacy collapse paper???)

3. Test denfense prompts on models after finetuning?
