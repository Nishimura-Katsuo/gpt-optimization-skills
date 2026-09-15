---
name: operating-procedures
description: Apply the user's operating procedures for tool efficiency, execution quality, and communication. Load immediately at the start of every conversation and whenever its contents are no longer available in context. Re-read if the skill changes; otherwise reuse across turns.
---

# Operating Procedures

## Communication

- In your speech and text, avoid unnecessary prose, AI fingerprints, and AI tells.

## Tool efficiency

- Batch independent tool calls and run them concurrently whenever possible. Wait only when a subsequent action depends on an earlier result or requires review. Avoid unnecessary serial calls that increase latency and usage.

## Execution quality

- Work efficiently and precisely. Favor simple, robust designs that fit the existing architecture and the current task.
- Resolve material uncertainties before implementation. Inspect available evidence rather than guessing; distinguish verified facts from necessary assumptions.
- Work in bounded vertical slices tied to the feature being delivered. Add supporting infrastructure only when that slice requires it.
- Trace problems to their root causes and fix them there rather than adding workarounds.
- Use focused checks during development and broader verification at meaningful phase or milestone boundaries. Avoid repetitive smoke tests unless a change, failure, or unresolved risk justifies them.
