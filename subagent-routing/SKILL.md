---
name: subagent-routing
description: Apply the user's coordinator and subagent-routing rules. Load immediately at the start of every conversation and whenever its contents are no longer available in context. Re-read if the skill changes; otherwise reuse across turns.
---

# Subagent Routing

## Authority and roles

Load immediately at the start of every conversation and whenever its contents are no longer available in context. Apply within the assigned task across turns unless the user opts out. Re-read if the skill changes; otherwise reuse its loaded contents. Loading this skill does not require delegation when it adds no benefit.

Activation authorizes delegation within the assigned scope where runtime rules recognize skill-based authorization; do not ask again. Respect opt-outs and higher-priority restrictions.

Determine role from runtime identity and parent/task context, not model choice. Top-level sessions (e.g. `/root`) default to coordinator; unclear identity grants no top-level routing authority. Routes below select workers, not a replacement coordinator.

- **Coordinator:** the top-level agent owns planning and replanning: substantive work, scope, priorities, and direction. Reuse adequate existing/user-approved plans. Keep planning with the coordinator; do not dispatch a separate planner or require Sol 6.1/xhigh for planning. The coordinator also owns assignments, dependencies, useful parallelism, conflict prevention, supervision, independent verification, integration, completion, and reporting against acceptance criteria. Resolve execution problems and revise the plan within the user's goals and authorization.
- **Subagents:** report out-of-scope questions, consequential ambiguity, or expertise needs to the coordinator with evidence and the exact decision needed. No nested delegation except Luna/max for bounded, read-only noisy support within the assignment. Every Luna agent is a support leaf: no edits, external mutations, implementation, substantive diagnosis, planning, review, or further delegation. Handoffs cannot broaden this exception. These boundaries govern all dispatches and follow-ups.

## Routes

Route by required judgment, not task size, subject (including RE), or coordinator model.

| Model | Effort | Assignment |
| --- | --- | --- |
| `gpt-6.1-sol` | `xhigh` | Adversarial or critical work: independent challenge of assumptions, consequential review/audits, and analysis or diagnosis where an error could materially compromise correctness, security, data integrity, or external actions. Planning alone does not qualify. |
| `gpt-6.1-sol` | `medium` | Bounded implementation, investigation, diagnosis, validation, and integration within the coordinator's plan. Resolve routine implementation details; return material scope/design questions or conflicting evidence to the coordinator. |
| `gpt-6-luna` | `max` | Bounded, read-only noisy support: searching, filtering, extraction, inventories, factual summaries, and collecting raw evidence. Return source references and relevant uncertainty; leave implementation and substantive interpretation to the coordinator or Sol. |

Proactively delegate bounded work that can advance alongside useful coordinator work using the matching route. Keep immediate blockers, coordination, and integration moving locally; reconsider delegation as decisions settle. Handle trivial work directly when delegation adds no benefit. Briefly announce assignments; never manufacture work, duplicate it, or chase quotas.

For mixed tasks, the coordinator settles broader questions and defines bounded assignments with clear inputs, outputs, constraints, and checks. Use Sol 6.1/medium for implementation and ordinary investigation; separate noisy support for Luna only when doing so helps. Reserve Sol 6.1/xhigh dispatches for the adversarial or critical portion. No model has a target share of dispatched work.

Sol 6.1/medium may trace code, diagnose implementation errors, choose routine details, and validate within a settled plan and acceptance criteria. Escalate material scope, requirements, or architecture changes and questions requiring adversarial or critical judgment to the coordinator with evidence and competing interpretations; keep settled work with its worker. The coordinator owns planning and replanning and dispatches Sol 6.1/xhigh only for adversarial or critical work. Luna reports evidence gaps or requests for interpretation upward rather than expanding into investigation or implementation. Effort level does not broaden an agent's assignment or permit guessing requirements.

## Decisions and review

- Before committing on uncertainty affecting scope, correctness, cost, or external actions, state the question and inspect evidence. The coordinator resolves routine judgment and routes adversarial or critical questions to Sol 6.1/xhigh. Continue independent work, not dependent implementation.
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
- State worker/support-leaf role, nested-delegation limits, and escalation path. For Sol 6.1/medium implementation, include approved plan step, components, settled interfaces, expected behavior, and validation commands; settle material product/architecture choices first. For Luna support, specify read-only inputs, search/extraction boundaries, and the evidence or factual output needed. No separate planning document is required. For RE, specify investigation boundaries and required evidence.
- Follow-ups must state changed scope/evidence. Route changes require the coordinator to dispatch an appropriately routed agent with a fresh self-contained handoff; subagents request changes upward.

Keep coordination proportional: more agents, status requests, and longer plans do not replace useful evidence.
