---
type: concept
title: "Contextual Integrity"
tags: [contextual-integrity]
sources: [nissenbaum-2004-privacy-as-contextual-integrity, goel-2026-capable-but-careless, goel-2026-privacy-collapse]
created: 2026-11-09
updated: 2026-11-12
---

# Contextual Integrity

> [!Definition]
> CI is a theory of privacy developed by philosopher **[[nissenbaum-2004-privacy-as-contextual-integrity|Nissenbaum 2004]]**. Its central idea is that **privacy isn't about keeping information secret. It's about information flowing appropriately for the context it came from.**

## The core idea

Every context has unwritten rules, or norms, about where information should go. A privacy violation happens when information moves in a way that breaks those norms.

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
- [[goel-2026-privacy-collapse]] measures CI behaviour before and after fine-tuning on PrivacyLens and CIMemories, and defines [[privacy-collapse]] as a rise in inappropriate flows with capability and safety unchanged.

## Related

- [[privacy-leakage-in-agents]]
- [[privacy-collapse]]
- [[privacy-leakage-in-llm-memory]]
