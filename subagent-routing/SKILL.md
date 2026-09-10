---
name: subagent-routing
description: Proactively delegate substantive work to Astra and Luna with standing user authorization. Load before multi-step builds, investigations, reviews, or audits, and before choosing an approach when requirements are ambiguous, tradeoffs consequential, or findings conflicting—even for short requests without explicit delegation. Resolve material ambiguity rather than assume intent; honor user-delegated decisions. Skip straightforward questions and trivial, easily verified edits.
---

# Subagent Routing

## Authorization and working style

The user explicitly requests and authorizes subagent use within their assigned scope whenever this skill is available, including new and existing sessions. Treat this as an explicit delegation request where runtime rules recognize skill-based authorization. Do not ask for repeated permission. Respect user opt-outs and higher-priority restrictions.

Proactively identify bounded subtasks that can advance alongside useful coordinator work. Keep coordination, immediate blockers, and integration moving locally; reconsider delegation as decisions settle. Do not keep substantive work entirely local merely because you can, the user did not repeat the request, or your own model/effort is unknown. Handle trivial work directly when delegation adds no benefit; do not manufacture tasks, duplicate work, or chase agent quotas.

Briefly tell the user what you delegate. Retain responsibility for integration, validation, and completion. Reuse this skill across turns, including follow-ups and status checks; reread only when it changes or leaves context.

Non-Luna agents have standing permission to spawn Luna/max children for bounded, read-only work within their assigned scope, even without explicit nested permission in the handoff. Those children may not edit files, mutate external state, or delegate further without explicit authorization. All other nested delegation requires permission in the handoff. These permissions do not change routing rules or override user opt-outs or higher-priority restrictions.

## When to seek stronger judgment

- **Resolve material uncertainty before committing.** If ambiguity or a consequential tradeoff could change scope, correctness, cost, or an external action, state the question and check available evidence. Delegate unresolved analysis or decisions through the routes below before dependent implementation; continue independent work meanwhile. Ask the user for unavailable intent, preferences, constraints, or institutional knowledge. If the user explicitly delegates a decision, choose within their goals and constraints, use the applicable analysis route, and explain consequential choices without asking them to decide again. Delegated discretion neither supplies missing facts nor expands permissions; agent agreement cannot establish user intent. Routine, reversible details with no material effect can use reasonable defaults, making relevant assumptions explicit.
- **Reassess when evidence changes.** Conflicting findings, repeated unsuccessful fixes, unexpected results, or completion claims without appropriate validation warrant reassessment. Provide the relevant agent with what was tried, what happened, and the unresolved question; ask for a discriminating check or revised approach. Do not continue dependent work on a disputed premise.
- **Review consequential or complex work independently.** Use a reviewer who did not produce the solution. Provide the original request, constraints, result, and validation evidence; ask for unsupported assumptions, uncovered requirements, and concrete defects. Resolve and verify actionable findings or report what remains unresolved. Skip separate review for trivial, easily verified work.

## Routes

Choose routes by the judgment required, not task size or the coordinator's model. Apply the RE exception below first, then Astra/medium for judgment, Luna/max for settled execution, and Astra/low for other work.

| Model | Effort | Use for |
| --- | --- | --- |
| `gpt-6-astra` | `medium` | Analysis, review, audits, planning, decisions, architecture, diagnosis, and resolving ambiguity or conflicting evidence outside reverse engineering. Applies even when the judgment task is brief, routine, noisy, or tedious. |
| `gpt-5.6-luna` | `max` | Well-defined implementation; mechanical, noisy, or tedious execution; extraction, inventories, factual summaries; and bounded reverse-engineering investigations, including their local reasoning. |
| `gpt-6-astra` | `low` | Otherwise-Astra/medium work directly related to reverse engineering, plus remaining work, including coordination and integration of settled work. |

Define tasks so most dispatched work can run on Luna/max. For mixed work, use Astra to settle the broader questions and define bounded Luna tasks with clear inputs, outputs, constraints, and acceptance checks. Do not send the whole task to Astra because one part needs judgment, or fragment it merely to increase Luna usage. Large mechanical tasks do not require Astra because of their size.

### Reverse engineering (RE)

Luna/max owns a bounded RE investigation: a concrete question, component, or batch with identified inputs and an evidence-based deliverable. Its scope includes local analysis, inference, hypothesis testing, and synthesis; the answer need not be known in advance. Difficulty, volume, task vocabulary, or the presence of reasoning does not alone justify moving it to Astra.

Examples include tracing behavior and dependencies; identifying symbols, wrappers, or library patterns; inferring types, layouts, calling conventions, formats, or protocols; comparing versions; and annotating disassembly or applying authorized renames and edits.

Use Astra/low for broader RE decomposition and planning, cross-investigation synthesis, reviews, audits, product or architectural decisions, and ambiguity beyond the assigned investigation. Do not relabel ordinary tracing or evidence checking as review to bypass Luna. Escalate only the unresolved question with evidence and competing interpretations; keep the remaining bounded work with Luna. Mark inferred names/types as tentative and distinguish observations from hypotheses.

All directly RE-related work that would otherwise route to Astra/medium uses Astra/low, including escalations and nested delegation. Outside RE, Luna must escalate judgment beyond its assigned execution scope to Astra/medium; Astra/low must route analysis, review, audits, planning, and new decisions to Astra/medium. Luna's max effort means thorough coverage, not permission to guess requirements.

## Dispatch and handoffs

- Use `agent_type: default` and no inherited conversation history (`fork_turns: none`, or `fork_context: false` where supported).
- Explicitly select the exact routed model and effort. If unavailable, report it rather than substitute another model.
- Give each agent one bounded task and a unique, descriptive name in its message; use `task_name` if supported.
- Supply a self-contained handoff with only relevant context:
  - Objective, scope, exact inputs or paths, and expected deliverable.
  - Evidence, settled decisions, assumptions, and unresolved questions.
  - Constraints, applicable instructions, authorized actions, editable files, and preservation of unrelated changes in the shared workspace.
  - Observable acceptance checks and stopping or escalation conditions.
- For Luna implementation, include the approved plan step, affected components, settled interfaces, expected behavior, and validation commands. Resolve material product or architectural decisions first; no separate planning document is required.
- For RE, make investigation boundaries and required evidence explicit. For non-Luna agents, carry forward their read-only Luna/max nesting permission and applicable routes.
- For follow-ups, provide changed scope and new evidence explicitly. If a different route is required, spawn an appropriately routed agent with a fresh, self-contained handoff.

Keep coordination proportional to the task. Additional agents, status requests, and longer plans do not substitute for useful evidence.
