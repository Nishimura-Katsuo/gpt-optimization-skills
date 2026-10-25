# Workspace Skills

Personal Codex skills for execution quality, subagent routing, and avoiding unnecessary reviewer expense.

## Operating procedures

[`operating-procedures`](./operating-procedures/) gives baseline guidance on understanding requests, execution quality, verification and reporting, safe actions, efficient tool use, communication, and creative quality. Explicit instructions from the user, system, or repository take precedence.

It loads at the start of every conversation and whenever its contents are no longer available in context. Agents check results against every part of the request, inspect evidence and existing code before changing anything, and confirm that the APIs they rely on exist. They make the smallest complete fix at the root cause without weakening checks, change approach after repeated failures, and report what was and wasn't verified. They protect existing work and confirm before hard-to-reverse or external actions.

## Subagent routing

[`subagent-routing`](./subagent-routing/) defines coordinator ownership, model selection, isolated handoffs, review, and patient supervision of subagents. It loads at the start of every conversation and whenever its contents are no longer available in context.

| Model | Reasoning | Work |
| --- | --- | --- |
| `gpt-6.1-sol` | `xhigh` | Adversarial or critical analysis and consequential review. Planning alone does not qualify. |
| `gpt-6.1-sol` | `medium` | Bounded implementation, investigation, diagnosis, validation, and integration. |
| `gpt-6-luna` | `max` | Bounded, read-only searching, extraction, inventories, and factual summaries. |

The coordinator retains planning, scope, integration, and completion. Activation authorizes delegation within the assigned scope where runtime rules recognize skill-based authorization, while respecting opt-outs and higher-priority restrictions. It encourages useful parallel work and keeps trivial work local. Subagents may delegate only bounded, read-only noisy support to Luna; Luna agents cannot delegate further.

## Avoid Auto-review

[`avoid-auto-review`](./avoid-auto-review/) warns when Auto-review is enabled because it can add avoidable reviewer-agent usage and expense.

It triggers when the effective runtime explicitly reports `approvals_reviewer: auto_review`, or when the user is considering enabling Auto-review. It does not trigger based on UI appearance or sandbox restrictions alone.

When Auto-review is active, it recommends switching to a suitable alternative and pauses once for the user's choice. If the user chooses to continue, work resumes without repeating the warning during the same task.

## License

[MIT](./LICENSE).
