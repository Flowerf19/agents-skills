---
name: implementation-planner
description: Compare designs and plan code changes using the current project. Use when the user asks to plan a feature, choose an architecture, turn a spec into tasks, or plan a complex approved change. Planning does not give permission to implement.
argument-hint: Feature, design question, bug report, or existing plan.
---

# Implementation planner

Recommend a design or produce a plan another developer can follow.

Plan only. Do not edit code, tests, configuration, dependencies, or unrelated guidance. For a discussion, answer in the conversation. Write a plan file only when requested or needed for an approved implementation.

## Step 1: Understand the change

1. Read the request, project instructions, affected code, and tests.
2. Record the source branch or commit. Check relevant project guides and existing decisions against the code.
3. State the goal, required behavior, constraints, and work outside the scope.
4. Follow the affected flow from its starting point to its caller. Find the modules that create and use the data. Find where shared types are defined and where dependencies are connected.
5. If a bug's cause is unclear, use `debug-investigator` first. Skip formal planning for a small, clear change.

## Step 2: Resolve design questions

1. Compare useful options for correctness, compatibility, cost, and testing.
2. Recommend one. Keep proposals separate from accepted decisions.
3. Ask about missing business rules or choices that change the approved scope. Keep blocking questions open until answered.

## Step 3: Write the needed plan

Use the existing plan format. If none exists, put an approved plan file in `.agents/plans/<slug>.md`. Create only the folder needed for that file.

Include:

- Goal, approved scope, and checks that show success.
- Accepted decisions and their sources; open questions separately.
- Small tasks, affected files or modules, and prerequisites. Follow dependencies, not task-number order.
- Required checks before starting the next phase or declaring the work complete.
- Test commands, expected results, failure cases, and required environment, services, models, or files. Separate unit tests, integration checks, and checks of the actual product flow. State what remains unverified if a required check cannot run.

Add details only when relevant:

- Interface changes: inputs, outputs, errors, callers, and behavior that must stay the same.
- Refactors: public imports, API payloads, event order, and stored-data compatibility.
- State changes: cancellation, concurrent updates, restart, migration, and rollback.
- UI changes: routes, shared components, and current design tokens.
- Benchmarks or classifiers: agreed metrics, representative cases, and counterexamples. Do not invent score thresholds.

## Step 4: Check and hand off

Check that each task has its prerequisites and a way to verify it. A plan with blocking questions is not ready to execute.

Return the recommendation or plan path, supporting evidence, open questions, and whether implementation is approved. Pass the plan to `thoughtful-coder` only when implementation is approved.

## Plan status

Use the project's format. Without an existing format, use `status`, `created`, and `last_updated` in YAML frontmatter.

| Status | Meaning |
|---|---|
| `draft` | Proposed work; execution is not yet approved |
| `in-progress` | Approved execution has started |
| `done` | Approved tasks are implemented and verified |
| `abandoned` | Replaced or stopped; record the reason |

Preserve task IDs and completed history. Update `last_updated` when the plan changes. Record scope changes explicitly. An accepted design can appear in a draft plan; it does not grant execution permission. Commit, merge, and deleting a plan still need their own permission.
