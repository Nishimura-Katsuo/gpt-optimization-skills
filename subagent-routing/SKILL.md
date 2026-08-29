---
name: subagent-routing
description: Use once per turn, before the first Codex subagent action, when the turn will involve delegation, parallel agent work, exploration, planning, review, coordination, follow-up messages, or waits. Route every subagent through one of three plain default model and reasoning configurations with isolated manually curated context.
---

# Subagent Routing

Load this skill once per turn, before the first subagent action, and apply its instructions throughout all subagent orchestration in that turn. Do not reload it before each spawn, follow-up, message, interrupt, status check, or wait. This skill selects a model and reasoning effort; it does not authorize delegation. Spawn subagents only when the user or an applicable project or skill instruction authorizes or requires them.

## Required invariants

- Always use `agent_type: default`.
- Always use `fork_turns: none`. Do not pass inherited conversation turns.
- Write a complete, manually curated `message` for every spawn. Include the objective, scope, exact inputs or paths, relevant evidence, decisions already made, constraints, authorized actions, required output, acceptance checks, and stopping conditions.
- Do not broaden the user's authorization or override applicable system, project, or skill instructions.
- Assume agents share the workspace. Identify files they may edit and tell them to preserve unrelated user changes.
- Do not authorize nested delegation unless the user or an applicable instruction specifically calls for it.
- Use the exact route selected below. Do not silently substitute a model or reasoning effort if it is unavailable.

## Route selection

Choose by task shape, not by output length or the parent agent's model.

### Sol on xhigh: difficult judgment and ambiguity

Use `gpt-5.6-sol` on `xhigh` when the subtask needs the highest level of critical thinking, has a very long agentic horizon, contains meaningful ambiguity, is not yet well-defined, or must create or substantially revise a plan. This is the default route for architecture, difficult diagnosis, uncertain tradeoffs, synthesis across conflicting evidence, high-consequence review, and most original planning.

Use this route whenever hidden dependencies, unclear success criteria, or consequential assumptions require the agent to reason through uncertainty. A large but purely mechanical search does not qualify by size alone.

```yaml
task_name: [task name]
agent_type: default
model: gpt-5.6-sol
reasoning_effort: xhigh
fork_turns: none
message: [manually curated instructions]
```

### Sol on medium: well-defined short-horizon coordination

Use `gpt-5.6-sol` on `medium` for short-horizon coordination or integration requiring limited critical thinking when the project, plan, architecture, interfaces, and acceptance criteria are already well-defined. Good uses include coordinating multiple implementation workstreams, sequencing an approved plan, reconciling predetermined outputs, or combining specified steps without reopening settled design decisions.

Do not use this route to invent the plan it is coordinating. Defer a single, tightly bounded implementation slice to Luna/max when it can be handed off completely. Escalate to Sol/xhigh if requirements conflict, important decisions are missing, or the agent must make architectural judgments.

```yaml
task_name: [task name]
agent_type: default
model: gpt-5.6-sol
reasoning_effort: medium
fork_turns: none
message: [manually curated instructions]
```

### Luna on max: exhaustive, unambiguous execution

Use `gpt-5.6-luna` on `max` for limited agentic tasks that require almost no discretionary judgment but benefit from exhaustive coverage or focused execution. Every input, boundary, operation, and output format must be extremely well-defined with no material ambiguity. Good uses include targeted codebase exploration, searching a known corpus, extracting facts, summarizing supplied material, inventorying files or symbols, and other simple tasks that produce or consume substantial noisy evidence.

Also use this route for very well-defined code implementation when an approved plan is already in place and the implementation slice can be handed off reasonably as a self-contained task. Specify the relevant plan step, exact scope and files or components, settled interfaces and constraints, expected behavior, acceptance criteria, and validation commands. The Luna subagent may implement and verify that bounded slice, but it should not redesign the plan or resolve consequential ambiguity.

Here, `max` is for thoroughness and coverage, not permission to infer missing requirements. If the agent would need to choose among interpretations, resolve a product or architectural tradeoff, or decide what matters without explicit criteria, use Sol/xhigh instead. If a mechanical task encounters unexpected ambiguity, have it report the ambiguity and evidence rather than guessing.

Give each Luna subagent one concrete, bounded task and a unique descriptive `task_name`.

```yaml
task_name: [unique bounded task name]
agent_type: default
model: gpt-5.6-luna
reasoning_effort: max
fork_turns: none
message: [manually curated instructions]
```

## Curated message shape

Supply only the context the subagent needs, but make that context self-contained:

```text
Objective:
Scope and exact paths:
Inputs and evidence:
Decisions already made:
Constraints and applicable instructions:
Authorized actions and prohibited actions:
Required output or artifact:
Acceptance checks:
Stopping and escalation conditions:
```

For Luna/max, every field should be concrete enough that the agent can proceed without interpreting intent. For Luna implementation, include the exact plan step, affected files or components, expected behavior, and validation. For Sol/medium, the plan and design decisions must already be settled. Otherwise use Sol/xhigh.
