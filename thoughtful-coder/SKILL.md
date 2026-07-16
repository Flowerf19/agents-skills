---
name: thoughtful-coder
description: Implement careful, surgical code changes with minimal scope, repository consistency, and focused verification.
argument-hint: A coding task, bug, refactor, or feature to implement carefully.
---

Use this skill to implement code changes. Optimise in this order: Correctness → Minimal diff → Consistency with repo → Verifiable outcome → Simplicity. Follow the shared work principles in `~/.claude/skills/AGENTS.md`; this file defines the coding-specific workflow.

## Before editing

- If `.agents/` is missing, run `architecture-docs` first to bootstrap it.
- Read project instructions and the code path being changed.
- Confirm runtime constraints from manifests and config when they affect the implementation.
- For bugs, reproduce or use a confirmed `debug-investigator` handoff before editing.

## Implementation

- Change the correct ownership point, not only the reported symptom path.
- Match existing style, helpers, and module boundaries.
- Clean only unused imports or artifacts introduced by the change.
- Add focused tests for changed behavior; scale broader verification with blast radius.
- Comment only non-obvious logic, briefly, directly above the relevant code.

Identify the behavior, make the smallest complete change, then run the narrow check and relevant broader tests. Do not stop at a proposal unless the user asked for one.

## Plan close-out

If the task has an implementation plan, tick completed tasks with the date and update its status:

- `done` - all approved work landed.
- `in-progress` - approved tasks remain.
- `abandoned` - superseded; record the reason.

Do not leave a completed plan in `in-progress`.

## Documentation impact

For non-trivial changes, report whether root `README.md`, `.agents/` docs, or a plan lifecycle changed. Defer broad documentation work to `architecture-docs`.

## Output

Lead with what changed and whether verification passed. Then state assumptions and unresolved risk only when they matter.
