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

Choose by the judgment required, not output size or the parent model.

| Model | Effort | Use for |
| --- | --- | --- |
| `gpt-6-astra` | `xhigh` | Ambiguity, architecture, difficult diagnosis, consequential tradeoffs, conflicting evidence, high-consequence review, long-horizon reasoning, or creating and substantially revising plans. |
| `gpt-6-astra` | `low` | Short-horizon coordination or integration with settled plans, interfaces, design decisions, and acceptance criteria. |
| `gpt-5.6-luna` | `max` | Unambiguous searches, extraction, inventories, summaries, or a self-contained implementation slice from an approved plan. |

Use Luna/max for a fully specified execution task; use Astra/low to coordinate settled work. Neither route should invent a plan or resolve consequential ambiguity. Route those decisions to Astra/xhigh. Large mechanical tasks do not need Astra solely because of their size.

If Luna or Astra/low encounters conflicting requirements or missing decisions, have it report the ambiguity and evidence for Astra/xhigh to resolve. Luna's `max` effort means thorough coverage, not permission to guess requirements.

## Message checklist

Include only relevant context, but make the handoff self-contained:

- Objective, scope, and exact paths or inputs.
- Relevant evidence and settled decisions.
- Constraints, applicable instructions, and authorized actions.
- Required output, acceptance checks, and stopping or escalation conditions.

For Luna implementation, include the approved plan step, affected files or components, settled interfaces, expected behavior, and validation commands. Leave no material product or architectural decisions open.

For follow-ups, supply changed scope and new evidence explicitly; do not assume the agent sees the parent conversation. If the task now requires a different route, spawn a correctly routed agent with a fresh, self-contained handoff.
