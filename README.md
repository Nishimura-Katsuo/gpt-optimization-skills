# Workspace Skills

## Subagent routing

[`subagent-routing`](./subagent-routing/) chooses the appropriate model and reasoning level for subagent work while ensuring each agent receives isolated, purpose-built context.

It triggers before any subagent is used, including delegation, exploration, planning, review, coordination, follow-up work, or waiting on an agent. It governs routing but does not authorize delegation.

## Avoid Auto-review

[`avoid-auto-review`](./avoid-auto-review/) warns when Auto-review is enabled because it can add avoidable reviewer-agent usage and expense.

It triggers when the effective runtime explicitly reports `approvals_reviewer: auto_review`, or when the user is considering enabling Auto-review. It does not trigger based on UI appearance or sandbox restrictions alone.
