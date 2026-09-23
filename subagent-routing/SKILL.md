---
name: subagent-routing
description: Apply the user's coordinator and subagent-routing rules only when the user explicitly invokes $subagent-routing or asks to use this skill.
---

# Subagent Routing

## Authority and roles

Activate only when the user explicitly invokes $subagent-routing or asks to use this skill. Availability, prior use in another task, or a request to inspect or edit the skill does not activate it. Once invoked, apply it within the assigned task across turns unless the user opts out. Re-read if the skill changes; otherwise reuse its loaded contents.

Explicit invocation authorizes delegation within the assigned scope where runtime rules recognize skill-based authorization; do not ask again. Respect opt-outs and higher-priority restrictions.

Determine role from runtime identity and parent/task context, not model choice. Top-level sessions (e.g. `/root`) default to coordinator; unclear identity grants no top-level routing authority. Routes below select workers, not a replacement coordinator.

- **Planner:** chooses substantive work, scope, priorities, and direction. Reuse adequate existing/user-approved plans. When planning or replanning is needed, reuse the Astra/medium planner or dispatch one. Straightforward questions and trivial edits need no planner.
- **Coordinator:** owns assignments, dependencies, useful parallelism, conflict prevention, supervision, independent verification, integration, completion, and reporting against acceptance criteria. Resolve execution problems within the plan; return evidence requiring a new direction to the planner rather than silently choosing different work.
- **Subagents:** report out-of-scope questions, consequential ambiguity, or expertise needs to the coordinator with evidence and the exact decision needed. No nested delegation except Luna/max for bounded, read-only noisy support (searching, filtering, extraction, inventories) within the assignment—not judgment, planning, review, or the substantive investigation. Support children are leaves: no edits, external mutations, or further delegation. Handoffs cannot broaden this exception. These boundaries govern all dispatches and follow-ups.

## Routes

Route by required judgment, not task size, subject (including RE), or coordinator model.

| Model | Effort | Assignment |
| --- | --- | --- |
| `gpt-6-astra` | `medium` | Analysis, review, audits, planning, decisions, architecture, diagnosis, and resolving ambiguity/conflicting evidence—even when brief, routine, noisy, or tedious. |
| `gpt-5.6-luna` | `max` | Well-defined implementation; mechanical/noisy/tedious execution; extraction, inventories, factual summaries, and simple assembly. |
| `gpt-6-astra` | `low` | Bounded integration of already-reviewed outputs: reconcile dependencies/interfaces within the approved plan, without new scope or design. Unresolved design or conflicting evidence goes to Astra/medium. |

Proactively delegate bounded work that can advance alongside useful coordinator work. Keep immediate blockers, coordination, and integration moving locally; reconsider delegation as decisions settle. Do not keep substantive work local merely because you can, authorization was not repeated, or your model/effort is unknown. Handle trivial work directly when delegation adds no benefit. Briefly announce assignments; never manufacture work, duplicate it, or chase quotas.

Shape most dispatched work for Luna. For mixed tasks, Astra/medium settles broader questions and defines bounded execution with clear inputs, outputs, constraints, and checks. Neither send the whole task to Astra for one judgment question nor fragment it to inflate Luna usage; volume alone does not require Astra.

Luna may trace code, diagnose implementation errors, choose routine details, and validate within a settled plan and acceptance criteria. Reasoning alone requires no escalation; independent planning/review, open-ended investigation, or scope/requirements/architecture changes do. Escalate only the unresolved question with evidence and competing interpretations through the coordinator to Astra/medium; keep settled work with its worker. Max effort is not permission to guess requirements.

## Decisions and review

- Before committing on uncertainty affecting scope, correctness, cost, or external actions, state the question, inspect evidence, and route unresolved judgment. Continue independent work, not dependent implementation.
- Ask the user for unavailable intent, preferences, constraints, or institutional knowledge. Honor explicitly delegated decisions within their goals, using the applicable route and explaining consequential choices without asking them to decide again. Discretion supplies neither missing facts nor extra permissions; agent agreement cannot establish user intent. Routine, reversible, immaterial details may use reasonable defaults with relevant assumptions stated.
- Reassess conflicting findings, repeated failed fixes, unexpected results, or unsupported completion claims. Give the relevant agent attempts, outcomes, and the unresolved question; request a discriminating check or revised approach. Do not proceed on disputed premises.
- Independently review consequential/complex work with someone other than its author. Supply the request, constraints, result, and validation evidence; check unsupported assumptions, missed requirements, and defects. Resolve and verify actionable findings or report what remains. Skip separate review for trivial, easily verified work.

## Patient supervision

Assume running agents are working. Duration, silence, missing interim files, token use, and repeated wait timeouts—even across turns—never alone justify interruption, termination, replacement, duplicate work, reduced scope, or marking the goal blocked. A timeout ends observation, not the task.

If concerned, ask non-interruptingly what the agent is doing, what it established, and whether a concrete blocker needs help. Allow time for reasoning/tools and a reply; do not demand immediate completion, placeholder proof of activity, or repeatedly nudge while a check-in is pending.

Use authoritative status and available waits; do useful independent work meanwhile. Preserve agents/context across turns and compactions. Judge evidence and acceptance criteria, not speed or update frequency.

Cancel/interrupt only for a concrete independent reason: user stop/scope change, observed unauthorized/harmful action, or confirmed failure requiring recovery. For reported blockers or suspected repetition, first request an explanation and help resolve it. Never manufacture failure by interrupting. Replace only after resolving ownership and confirming the original stopped.

## Handoffs

- Use `agent_type: default`, no inherited history (`fork_turns: none` or supported `fork_context: false`), and the exact routed model/effort. Report unavailable routes; do not substitute.
- Assign one bounded task with a unique descriptive name in the message (`task_name` when supported).
- Provide only relevant, self-contained context: objective, scope, exact inputs/paths, deliverable, evidence, settled decisions, assumptions, open questions, constraints/instructions, authorized actions/editable files, preservation of unrelated changes, observable acceptance checks, and stopping/escalation conditions.
- State worker/support-leaf role, nested-delegation limits, and escalation path. For Luna implementation, include approved plan step, components, settled interfaces, expected behavior, and validation commands; settle material product/architecture choices first. No separate planning document is required. For RE, specify investigation boundaries and required evidence.
- Follow-ups must state changed scope/evidence. Route changes require the coordinator to dispatch an appropriately routed agent with a fresh self-contained handoff; subagents request changes upward.

Keep coordination proportional: more agents, status requests, and longer plans do not replace useful evidence.
