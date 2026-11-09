---
type: concept
title: "Contextual Integrity"
tags: [contextual-integrity]
sources: [nissenbaum-2004-privacy-as-contextual-integrity]
created: 2026-11-09
updated: 2026-11-09
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

## Related

- [[privacy-leakage-in-agents]]
- [[privacy-leakage-in-llm-memory]]
