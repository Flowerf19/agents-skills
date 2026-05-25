---
name: implementation-planner
description: Turn specs, features, bugs, or subsystem goals into execution-ready implementation plans grounded in repo evidence.
argument-hint: Feature specification, issue description, or planning goal.
---

Use this skill to convert a spec/feature/bug/subsystem goal into a concise plan another agent can execute. Output: `implementation_plan.md` for user review. Planning-only — do not edit code or mutate project files.

## Priority

1. Stay planning-only.
2. Evidence over assumption: codegraph + repo files + `.agents/` guidance.
3. Decision-complete: small, testable tasks with exact commands and relevant files.

## Before drafting

- If `.agents/` is missing, run the `architecture-docs` skill first to bootstrap.
- Read instruction entrypoints only: `AGENTS.md`, `CLAUDE.md`, `.agents/README.md`, `.github/copilot-instructions.md`.
- Use codegraph for structure (definitions, callers, callees, impact, related files). Read source only after codegraph points at the exact target.
- Check official external docs only when the plan depends on third-party API/library behavior.

## Clarifications

Ask only when missing detail materially changes the plan. Safe defaults: proceed and list as an assumption. Spec conflicts with repo: name the conflict, propose a compatible alternative. Spec unsafe/impossible: stop and ask.

## Plan content

Include:
- Goal + success criteria
- Key changes by subsystem/behavior
- Interfaces, schemas, commands, or files only when ambiguity demands them
- Edge cases and failure modes affecting correctness
- Focused tests and acceptance criteria
- Assumptions and defaults

Avoid: broad rewrites, speculative abstractions, duplicated logic, TODO/TBD placeholders, file-by-file inventories. Do not dump call graphs or symbol lists — point at codegraph queries instead.

## Output

`implementation_plan.md` (default location: `.agents/plans/<slug>.md`) with a YAML header for lifecycle tracking:

```yaml
---
status: draft   # draft | in-progress | done | abandoned
created: YYYY-MM-DD
---
```

Body sections: Summary, Key Changes, Test Plan, Assumptions. Execution-ready.

## Lifecycle

- `draft` — plan written, waiting for user approval.
- `in-progress` — approved, `thoughtful-coder` is executing.
- `done` — implementation merged. Either keep as decision record in `.agents/plans/` with `status: done`, or move to `.agents/decisions/` if it captures a long-lived architectural choice.
- `abandoned` — superseded or no longer needed. Keep the file with `status: abandoned` and a one-line reason, so future planners don't re-derive the same dead end.

`thoughtful-coder` updates `status` at end of implementation; do not leave plans in `in-progress` after merging.
