---
name: subagent-routing
description: Proactively route substantive project work to the specified Astra and Luna subagents with standing user authorization. Load before planning or executing multi-step builds, implementations, investigations, reviews, or audits, not only after deciding to delegate. Reuse this skill across turns. Use default agents with manually curated context and no inherited turns.
---

# Subagent Routing

Apply throughout the conversation. Reuse loaded instructions across turns, including follow-ups, messages, status checks, and waits; these actions do not require another read. Re-read only if the skill changes or its contents are no longer available in context. Standing user authorization: In any Codex session where this skill is available, the user explicitly requests and authorizes subagent use by default within the scope of the work they assign. This authorization applies to new and existing sessions and does not need to be repeated in the session's messages. Treat this skill as an explicit request for subagents wherever runtime rules allow authorization through applicable skill instructions. Use subagents for useful, bounded subtasks that can run independently alongside other work, without asking for separate delegation approval. Handle trivial work directly when delegation would add overhead without benefit. Respect explicit user opt-outs and higher-priority restrictions. Non-Luna subagents always have standing authorization to spawn `gpt-5.6-luna` / `max` subagents for bounded, read-only work within their assigned scope, even when their handoff does not explicitly permit nested delegation. These Luna children must not edit files, mutate external state, or delegate further without explicit authorization. All other nested delegation requires explicit permission in the handoff. This exception changes delegation permission only; the routing responsibilities below, explicit user opt-outs, and higher-priority restrictions still apply.

## Delegation default

For substantive project work, actively identify useful bounded work for the routes below and dispatch it when it can advance alongside the coordinator's work. Delegation is the user's preferred working method, not merely an available option. Do not default to doing the whole project yourself because you can, because the user did not repeat the request for subagents, or because your own model or effort is unknown.

Keep coordination, immediate blocking work, and integration moving locally. Delegate independent planning or review to Astra at the applicable effort, and settled implementation or bounded investigations to Luna/max. Reconsider delegation as decisions settle and new independent tasks become available. Follow the RE exceptions below. Handle trivial tasks directly; do not manufacture subtasks or duplicate work to satisfy a delegation quota.

The routes specify the models and effort to request for subagents; they do not depend on identifying or changing the coordinator's model. Explicitly select the routed model and effort at spawn time. Briefly tell the user what is delegated and retain responsibility for integration, validation, and completion.

## Spawn rules

- Use `agent_type: default` and disable inherited conversation history using the current tool's supported parameter (`fork_context: false`, or `fork_turns: none` where supported).
- Give each agent one bounded task and a unique, descriptive task name in its message; use `task_name` only if the tool supports it.
- Supply a self-contained `message` using the checklist below. In handoffs to non-Luna subagents, carry forward their standing permission to use read-only Luna/max children and the applicable routing rules.
- Agents share the workspace. Specify which files they may edit and require preservation of unrelated changes.
- Use only the exact model IDs listed below and supported by the current `spawn_agent` tool. If the required model is unavailable, report it; do not substitute another model.

## Routes

Prefer defining tasks well enough that most dispatched work can run on Luna/max. Before routing a broad task, separate Astra responsibilities from Luna work using the routes below. Bounded reverse-engineering investigations stay with Luna/max, including their local analysis, inference, hypothesis checking, and synthesis; do not extract that reasoning into an Astra task. Use Astra/medium (Astra/low for directly RE-related work outside Luna's investigation scope) to settle broader questions and specify bounded Luna/max tasks with clear inputs, expected deliverables, constraints, and acceptance checks. Do not send an entire mixed task to Astra merely because one part needs judgment; isolate that part and return the resulting bounded work to Luna/max. Keep task boundaries useful rather than fragmenting work just to increase Luna usage.

Choose each resulting task's route by the judgment still required, not output size or the parent model. Apply the reverse-engineering carve-out before the general Astra/medium responsibilities below; Astra/low also remains the fallback for work outside the other two routes.

| Model | Effort | Use for |
| --- | --- | --- |
| `gpt-6-astra` | `medium` | Analysis, review, auditing, planning, decision making, architecture, diagnosis, and resolution of ambiguity or conflicting evidence, except work directly related to reverse engineering. |
| `gpt-5.6-luna` | `max` | Well-defined implementations, noisy tasks, and tedious tasks, including reverse engineering, mechanical searches, extraction, inventories, and factual summaries. |
| `gpt-6-astra` | `low` | All otherwise-Astra/medium work directly related to reverse engineering, plus everything outside the other two routes, including coordination or integration of settled work. |

Except for the reverse-engineering carve-out below, apply the Astra/medium rule first: analysis, review, planning, and decision making go there even when brief, routine, noisy, or tedious. Use Luna/max for the remaining well-defined implementation or noisy and tedious execution work. Use Astra/low for everything else. Large mechanical tasks do not need Astra solely because of their size.

### Reverse engineering (RE)

Default to Luna/max for any bounded RE investigation or well-defined execution task: a concrete question, component, or batch with identified inputs and an evidence-based deliverable. The answer need not be known in advance. Luna owns the local analysis, inference, hypothesis checking, and synthesis needed to complete that investigation. Route by scope, not task vocabulary; an unlisted task, technical difficulty, volume, or the presence of reasoning is not by itself a reason to use Astra.

Representative Luna/max work (not an exhaustive list):

- Identify and name functions, parameters, arguments, variables, and fields.
- Trace callers, callees, control flow, and data flow; reconstruct specific behaviors and algorithms.
- Map imports, exports, strings, constants, cross-references, interfaces, and dependencies.
- Identify wrappers, library functions, and repeated code patterns.
- Infer signatures, types, structures, field layouts, and calling conventions.
- Map file formats, protocol fields, and serialization routines.
- Annotate disassembly or decompiled code; apply settled renames, types, and other authorized edits.
- Compare versions, catalog changed functions, and summarize observed behavior with supporting evidence.

Use Astra/low for broader RE decomposition and planning, synthesis across investigations, reviews, audits, product or architectural decisions, and unresolved ambiguity beyond an assigned investigation. Do not relabel ordinary tracing, comparison, or evidence checking as a review or audit to bypass Luna. Isolate the part that requires Astra and return bounded follow-on work to Luna/max. All directly RE-related work that would otherwise use Astra/medium uses Astra/low, including escalations and nested delegation.

In RE handoffs, specify inputs, boundaries, and required evidence. Luna should distinguish observations from hypotheses, mark uncertain names or types as tentative, and escalate only the unresolved question with evidence and competing interpretations; uncertainty in one finding should not move the entire batch to Astra.

If Luna encounters a need beyond its bounded investigation or settled execution scope, have it report the relevant evidence to Astra/low when directly related to reverse engineering, otherwise Astra/medium. Outside work directly related to reverse engineering, Astra/low must likewise route analysis, review, auditing, planning, or new decisions to Astra/medium. Luna's `max` effort means thorough coverage, not permission to guess requirements.

## Message checklist

Include only relevant context, but make the handoff self-contained:

- Objective, scope, and exact paths or inputs.
- Relevant evidence and settled decisions.
- Constraints, applicable instructions, and authorized actions.
- Required output, acceptance checks, and stopping or escalation conditions.

For Luna implementation, include the approved plan step, affected files or components, settled interfaces, expected behavior, and validation commands. Leave no material product or architectural decisions open.

For follow-ups, supply changed scope and new evidence explicitly; do not assume the agent sees the parent conversation. If the task now requires a different route, spawn a correctly routed agent with a fresh, self-contained handoff.
