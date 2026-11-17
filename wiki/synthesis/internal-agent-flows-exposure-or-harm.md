---
type: synthesis
title: "Internal agent flows: exposure or harm?"
tags: [agents, contextual-integrity]
sources: [el-yagoubi-2026-agentleak, gupta-2026-pisas]
created: 2026-11-16
updated: 2026-11-17
---

# Internal agent flows: exposure or harm?

When one agent passes sensitive information to another agent inside the same system, is that a privacy violation? Filed from the discussion with Hamid on 2026-11-16 while reading [[el-yagoubi-2026-agentleak|AgentLeak]] against [[gupta-2026-pisas|PiSAs]].

> [!note] Hamid's view
> The real harm happens when an agent discloses private information to a user. Who cares what an agent knows internally?

## Short answer

Under [[contextual-integrity]] a flow needs a recipient. A worker agent in the same deployment, owned by the same party, is not a new recipient, so Hamid's view holds for that case. An internal flow matters when it is a step towards a recipient who should not have the information.

## AgentLeak and PiSAs both score internal messages

| | [[agentleak\|AgentLeak]] | [[pisas\|PiSAs]] |
|---|---|---|
| Who the agents stand for | Components of one enterprise deployment (coordinator and worker). Users and owners are not defined. | Each agent represents a different teammate, or one agent serves the whole team. |
| Why an internal message is a violation | A vault field outside the task's allowed set appears on a channel. No new person needs to see it. | An attribute the task does not need is gathered or sent, or an attribute reaches a teammate (or their agent's memory) who should not have it. |
| Norm | Data minimization, per channel. | Appropriateness (does the task need it?) and visibility (who may see it?). |
| Adversary | Benign plus two attacker levels. | None. |
| Systems | Two agents, coordinator-worker. | 5 to 10 agents; single, centralized, decentralized. |

AgentLeak's allowed set and PiSAs' appropriateness label ask the same question. PiSAs' visibility label is the part that ties an internal flow to a person, and AgentLeak has no counterpart to it.

## When an internal flow matters

- **The agent stands for someone else.** In PiSAs a message to another user's agent, or a write to its memory, counts as a visibility violation because it can surface to that user ([[gupta-2026-pisas]]).
- **The channel is not really internal.** AgentLeak classes tool inputs and logs as external: they go to third-party APIs and to whoever reads the telemetry ([[el-yagoubi-2026-agentleak]]).
- **It persists.** AgentLeak defines shared memory as state that "persists across execution boundaries". In PiSAs memory lasts one run, so neither paper tests a later session with a different recipient.
- **It widens the attack surface.** AgentLeak's "risk amplification" level: data in internal channels can be exfiltrated, logged or retained too long.
- **Regulation covers processing.** AgentLeak grounds its rule in HIPAA's minimum-necessary standard, GDPR Art. 25 and Quebec Law 25, under which the policy is broken "even if no outsider ever sees the data".

## What the two papers agree and disagree on

- **Agree:** splitting work across agents lowers what shows in the final output while opening internal surfaces. AgentLeak: output leakage 43.2% → 27.2%, any-channel 68.9%. PiSAs: memory raises completeness and "relocates" violations.
- **Agree:** no attacker is needed. AgentLeak's message leakage is 69.3% in the benign condition and about the same under attack; PiSAs has no adversary at all.
- **Disagree on the total.** AgentLeak says multi-agent raises total exposure; PiSAs says it lowers appropriateness violations (36.8% single → 19.0% decentralized). The baselines differ: a single agent in AgentLeak has no internal channels to leak on, while a single agent in PiSAs sees everything. Recorded, not resolved.
- **Defenses:** AgentLeak's output-level defenses leave internal leakage at about 30%. PiSAs' output-filter result (6%) is single-agent only, so it does not test internal channels.

## Related

- [[research-agenda]] (open questions on this topic)
- [[privacy-leakage-in-agents]]
- [[privacy-leakage-in-llm-memory]]
- [[prompt-defenses-for-contextual-privacy]]
