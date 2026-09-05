---
name: subagent-routing
description: Select Astra or Luna and reasoning effort for authorized Codex subagent work. Load once per turn before the first subagent action, including follow-ups, messages, status checks, or waits. Require default agents with manually curated context and no inherited turns.
---

# Subagent Routing

Apply throughout the turn. This skill selects routes; it does not authorize delegation or nested delegation. Follow the session's authorization rules.

## Spawn rules

- Use `agent_type: default` and `fork_turns: none`.
- Give each agent one bounded task and a unique, descriptive `task_name`.
- Supply a self-contained `message` using the checklist below.
- Agents share the workspace. Specify which files they may edit and require preservation of unrelated changes.
- Use only the exact model IDs listed below and supported by the current `spawn_agent` tool. If the required model is unavailable, report it; do not substitute another model.

## Routes

Prefer defining tasks well enough that most dispatched work can run on Luna/max. Before routing a broad task, separate the analysis, planning, review, and decisions from execution. Use Astra/xhigh to settle those questions and specify bounded Luna/max tasks with clear inputs, expected behavior, constraints, and acceptance checks. Do not send an entire mixed task to Astra merely because one part needs judgment; isolate that part and return the resulting execution work to Luna/max. Keep task boundaries useful rather than fragmenting work just to increase Luna usage.

Choose each resulting task's route by the judgment still required, not output size or the parent model. This preference does not override the Astra/xhigh responsibilities below; Astra/low remains the fallback for work outside the other two routes.

| Model | Effort | Use for |
| --- | --- | --- |
| `gpt-6-astra` | `xhigh` | All analysis, review, planning, decision making, architecture, diagnosis, and resolution of ambiguity or conflicting evidence. |
| `gpt-5.6-luna` | `max` | Well-defined implementations, noisy tasks, and tedious tasks, including reverse engineering, mechanical searches, extraction, inventories, and factual summaries. |
| `gpt-6-astra` | `low` | Everything else, including coordination or integration of settled work that does not require analysis, review, planning, or new decisions. |

Except for the reverse-engineering carve-out below, apply the Astra/xhigh rule first: analysis, review, planning, and decision making go there even when brief, routine, noisy, or tedious. Use Luna/max for the remaining well-defined implementation or noisy and tedious execution work. Use Astra/low for everything else. Large mechanical tasks do not need Astra solely because of their size.

Treat reverse engineering as tedious work for Luna/max. Break it into bounded questions or components that Luna can investigate, such as tracing a call path, mapping an interface or data format, or reconstructing a specific behavior. Include the relevant inputs, scope, and required evidence in each handoff. Luna may perform the local reasoning needed to answer those questions; do not escalate merely because reverse engineering involves analysis. Route decomposition planning, broader synthesis or review, and unresolved product or architectural decisions to Astra/xhigh.

Outside that bounded reverse-engineering work, if Luna or Astra/low encounters a need for analysis, review, planning, or a new decision, have it report the relevant evidence for Astra/xhigh to handle. Luna's `max` effort means thorough coverage, not permission to guess requirements.

## Message checklist

Include only relevant context, but make the handoff self-contained:

- Objective, scope, and exact paths or inputs.
- Relevant evidence and settled decisions.
- Constraints, applicable instructions, and authorized actions.
- Required output, acceptance checks, and stopping or escalation conditions.

For Luna implementation, include the approved plan step, affected files or components, settled interfaces, expected behavior, and validation commands. Leave no material product or architectural decisions open.

For follow-ups, supply changed scope and new evidence explicitly; do not assume the agent sees the parent conversation. If the task now requires a different route, spawn a correctly routed agent with a fresh, self-contained handoff.
