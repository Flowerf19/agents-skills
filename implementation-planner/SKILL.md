---
name: implementation-planner
description: Create or update execution-ready implementation plans from specs, features, bugs, or subsystem goals, grounded in repository evidence.
argument-hint: Feature specification, issue description, or planning goal.
---

Create or update an execution-ready plan another agent can follow. Planning-only: write the plan artifact, not runtime code or config.

## Before drafting

- If `.agents/` is missing, run `architecture-docs` first to bootstrap it.
- Read project instructions, the request, any existing plan, and relevant repo evidence.
- Resolve decisions that affect scope, interfaces, sequencing, or acceptance criteria.
- Ask only when no safe default exists; otherwise record the assumption.
- If the spec conflicts with the repo, name the conflict and propose a compatible alternative. Stop for unsafe or impossible requirements.

## Plan content

Include:

- Goal and measurable success criteria.
- Phase goals (`GOAL-NNN`) with small independently verifiable tasks (`TASK-NNN`).
- Interfaces, schemas, commands, and files only where needed to remove execution ambiguity.
- Correctness-relevant edge cases and failure modes.
- Focused tests and acceptance criteria.
- Assumptions and defaults.

Do not include broad rewrites, speculative abstractions, TODO/TBD placeholders, file inventories, or call-graph dumps.

## Format

Default path: `.agents/plans/<slug>.md`.

```yaml
---
status: draft
created: YYYY-MM-DD
last_updated: YYYY-MM-DD
---
```

Body sections: Summary, Tasks, Test Plan, Assumptions.

```md
### GOAL-001: <phase goal>

| ID | Task | Done | Date |
|----|------|------|------|
| TASK-001 | <small verifiable step> | | |
```

IDs are append-only; never renumber existing `TASK` or `GOAL` entries.

## Lifecycle

- `draft` - waiting for approval.
- `in-progress` - approved and being executed.
- `done` - implementation merged; keep as a record or move to `.agents/decisions/` for a lasting decision.
- `abandoned` - superseded or cancelled; keep a one-line reason.

Update existing plans in place. Keep completed IDs, append new IDs, strike superseded tasks with a reason, bump `last_updated`, and restore `in-progress` when approved work resumes.
