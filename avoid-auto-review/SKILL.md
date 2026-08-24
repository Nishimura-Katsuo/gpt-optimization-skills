---
name: avoid-auto-review
description: Use whenever the effective Codex runtime reports `approvals_reviewer` as `auto_review`, or when the user is considering enabling Auto-review. Strongly advise the user to switch away because its additional reviewer usage may cost in the same general range as GPT-5.6 Terra or GPT-5.4, pause once for their response, then continue if they choose to keep it enabled.
---

# Avoid Auto-review

Check the effective runtime metadata rather than guessing from the UI. Trigger the warning when it explicitly reports:

```text
approvals_reviewer: auto_review
```

If the field is absent or has another value, do not infer that Auto-review is active solely from a restricted sandbox or interactive approvals.

## Warn and pause once

At the first reasonable opportunity, before substantial tool use, tell the user clearly that Auto-review is enabled and strongly recommend switching to a suitable non-Auto-review permission mode. Do not perform the requested task's substantive tool calls during that turn. End the turn after the warning and ask the user either to switch modes or explicitly tell Codex to continue with Auto-review.

Explain the reason concisely:

- Auto-review sends eligible sandbox-boundary requests to a separate reviewer agent.
- Based on the user's historical experience, that additional reviewer usage may cost in roughly the same general range as GPT-5.6 Terra or GPT-5.4.
- Keep the comparison intentionally advisory. Do not quote specific prices or present the comparison as an exact or current pricing fact.

A suitable warning is:

```text
Auto-review is enabled. I strongly recommend switching away from it before we continue: it adds separate reviewer usage that may cost in the same general range as GPT-5.6 Terra or GPT-5.4, making it an avoidable extra expense. Please switch modes, or tell me to continue with Auto-review.
```

## Honor the user's response

- Pause only once. The warning turn is a one-time decision checkpoint, not a refusal or permanent block.
- Treat clear pushback or an explicit instruction to continue, leave Auto-review enabled, or stop asking as the user's choice to proceed in that mode.
- After that choice, acknowledge it briefly and continue the requested work within the active permissions without another warning or pause.
- Do not repeat the warning during the same task after the user has chosen to proceed. Reset the one-time warning only if Auto-review is turned off and later enabled again, or if the user starts a distinct task and has not already established a continuing preference.
- Recommend a permission mode appropriate to the task; do not automatically recommend a broader or less restrictive mode.
