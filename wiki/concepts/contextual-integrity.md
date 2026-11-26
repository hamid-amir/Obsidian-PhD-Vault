---
type: concept
title: "Contextual Integrity"
tags: [contextual-integrity]
area: [privacy-safety]
sources: [nissenbaum-2004-privacy-as-contextual-integrity, goel-2026-capable-but-careless, goel-2026-privacy-collapse, mireshghallah-2025-cimemories, gupta-2026-pisas, el-yagoubi-2026-agentleak, wang-2026-probing-steering-ci-norms]
created: 2026-11-09
updated: 2026-11-23
---

# Contextual Integrity

> [!Definition]
> CI is a theory of privacy developed by philosopher **[[nissenbaum-2004-privacy-as-contextual-integrity|Nissenbaum 2004]]**. Its central idea is that **privacy isn't about keeping information secret. It's about information flowing appropriately for the context it came from.**

## The core idea

Every context has unwritten rules, or norms, about where information should go. A privacy violation happens when information moves in a way that breaks those norms.

Under CI, privacy is not a property of data in isolation but of information flows: the same attribute can be appropriate to share in one task context and a violation in another

Take your medical diagnosis:

- You tell your **doctor**: fine. That's what the doctor context is for.
- Your doctor tells a **specialist** to help treat you: fine. It fits the norms of healthcare.
- Your doctor tells your **employer**: a violation, even though the information is the same.

## What defines a flow

Contextual integrity describes each information flow using a few parameters:

- **Sender:** who shares it (you, your doctor)
- **Recipient:** who receives it (a specialist, your employer)
- **Subject:** who the information is about (you)
- **Information type:** what kind of information it is (a medical diagnosis)
- **Transmission principle:** the conditions of sharing (in confidence, with consent, as required by law)

Change any one of these and a flow can go from appropriate to inappropriate.

## Operationalizing CI for evaluation

- [[goel-2026-capable-but-careless|AgentCIBench]] turns CI into a per-scenario label: for a given recipient and task, a set of items that must be shared and a set that must not. A leak is any must-not-share item in the agent's action. Its three failure modes each perturb one flow parameter (attributes surfaced, transmission principle, recipient). The authors note these labels are scenario-specific judgments that may vary by culture or organization.
- [[mireshghallah-2025-cimemories|CIMemories]] labels each (memory attribute, task + recipient) pair as share or withhold using GPT-5 under three Westin privacy personas, and scores only pairs where all personas agree. This handles disagreement about norms by discarding ambiguous flows.
- [[gupta-2026-pisas|PiSAs]] gives every attribute two labels: task-contextual appropriateness (should it be used for this task?) and per-person visibility (which teammates may see it?). Visibility is labelled by three LLM privacy personas, and a human check agreed with 86.5% of appropriateness labels and 80.6% of visibility labels.
- [[el-yagoubi-2026-agentleak|AgentLeak]] cites CI but operationalizes privacy as data minimization: a vault of sensitive fields and a small allowed set the task needs, labelled by two domain experts. The policy is per channel, not per recipient, and no sender, recipient or subject is defined. Its allowed set asks the same question as PiSAs' appropriateness label; it has nothing like the visibility label. See [[internal-agent-flows-exposure-or-harm]].
- [[goel-2026-privacy-collapse]] measures CI behaviour before and after fine-tuning on PrivacyLens and CIMemories, and defines [[privacy-collapse]] as a rise in inappropriate flows with capability and safety unchanged.
- [[wang-2026-probing-steering-ci-norms]] uses the flow parameters inside the model: synthetic examples that vary one parameter at a time (information type, recipient, transmission principle) yield separable activation directions, and steering along all three together cuts leakage, though at a large utility cost. Adding subject and sender as further axes did worse. See [[activation-probing-and-steering]].

## Related

- [[privacy-leakage-in-agents]]
- [[privacy-collapse]]
- [[privacy-leakage-in-llm-memory]]
