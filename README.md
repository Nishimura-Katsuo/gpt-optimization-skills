# Workspace Skills

Personal Codex skills for execution quality, subagent routing, and avoiding unnecessary reviewer expense.

## Operating procedures

[`operating-procedures`](./operating-procedures/) applies shared guidance for clear communication, creative quality, efficient tool use, and reliable execution.

It loads at the start of every conversation and whenever its contents are no longer available in context. It emphasizes inspecting evidence before making decisions, batching independent tool calls, resolving material uncertainty, and completing work in bounded slices with appropriate verification.

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
