---
name: implementation-planner
description: Create or update execution-ready implementation plans from specs, features, bugs, or subsystem goals, grounded in repo evidence.
argument-hint: Feature specification, issue description, or planning goal.
---

Use this skill to convert a spec/feature/bug/subsystem goal into a concise plan another agent can execute, or to update an existing plan when requirements change. Output: `implementation_plan.md` for user review. Planning-only — do not edit code or mutate project files.

## Priority

1. Stay planning-only.
2. Evidence over assumption: codegraph + repo files + `.agents/` guidance.
3. Decision-complete: small, testable tasks with exact commands and relevant files.

## Before drafting

- If `.agents/` is missing, run the `architecture-docs` skill first to bootstrap.
- Read instruction entrypoints only: `AGENTS.md`, `CLAUDE.md`, `.agents/README.md`, `.github/copilot-instructions.md`.
- Use codegraph for structure (definitions, callers, callees, impact, related files). Read source only after codegraph points at the exact target.
- When the plan depends on third-party API/library behavior, query Context7 MCP first; fall back to web search only if Context7 lacks the library.

## Clarifications

Ask only when missing detail materially changes the plan. Safe defaults: proceed and list as an assumption. Spec conflicts with repo: name the conflict, propose a compatible alternative. Spec unsafe/impossible: stop and ask.

## Plan content

Include:
- Goal + success criteria
- Key changes grouped into phase goals (`GOAL-NNN`), each broken into small, independently-verifiable tasks (`TASK-NNN`) — divide and conquer: a task too big to verify in one sitting must be split
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
last_updated: YYYY-MM-DD   # bump whenever the plan changes
---
```

Body sections: Summary, Tasks, Test Plan, Assumptions. Execution-ready.

The Tasks section uses stable IDs and a completion ledger, so executors can reference tasks precisely and updates can track them across revisions:

```md
### GOAL-001: <phase goal>

| ID | Task | Done | Date |
|----|------|------|------|
| TASK-001 | <small, verifiable step> | | |
| TASK-002 | <small, verifiable step> | | |
```

IDs are append-only — never renumber an existing `TASK`/`GOAL`; new work gets the next free number.

## Lifecycle

- `draft` — plan written, waiting for user approval.
- `in-progress` — approved, `thoughtful-coder` is executing.
- `done` — implementation merged. Either keep as decision record in `.agents/plans/` with `status: done`, or move to `.agents/decisions/` if it captures a long-lived architectural choice.
- `abandoned` — superseded or no longer needed. Keep the file with `status: abandoned` and a one-line reason, so future planners don't re-derive the same dead end.

`thoughtful-coder` ticks each `TASK` (`Done` ✅ + date) as it lands and updates `status` at end of implementation; do not leave plans in `in-progress` after merging.

## Updating a plan

When requirements change, update the existing plan in place — do not rewrite from scratch:

- Keep completed tasks and their IDs untouched as the record of what shipped.
- Add new requirements as new `TASK-`/`GOAL-` IDs (next free number). Mark superseded tasks struck-through with a one-line reason instead of deleting them.
- Bump `last_updated`; set `status` back to `in-progress` if approved work resumes.
