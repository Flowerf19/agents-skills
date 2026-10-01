---
name: thoughtful-coder
description: Make approved code changes and check that they work. Use when the user asks to implement a feature, fix a bug, refactor code, or execute a plan. Do not use for discussion, planning, or review-only requests.
argument-hint: Code change, approved plan, or confirmed bug fix.
---

# Thoughtful coder

Deliver the requested behavior, a focused diff, and evidence from relevant checks.

Edit only when implementation is approved. An accepted design or review suggestion alone is not permission. Complete the approved work; ask before a new decision changes its scope. Commit and merge need separate permission.

## Step 1: Understand before editing

1. Check existing changes and keep them separate from this task.
2. Read the request or accepted plan, project instructions, affected code, tests, and callers. Check the current configuration and relevant guides.
3. Follow the affected flow from its starting point to its result. Find the files responsible for its data, rules, and external calls.
4. For a bug, reproduce it and find its cause, or verify the evidence from `debug-investigator`. If a design question blocks the change, use `implementation-planner`.
5. Choose how to verify the requested behavior before editing.

## Step 2: Make the change

1. Reuse working project patterns. Make the smallest change that fully solves the task.
2. Keep business rules, parsing, workflow control, and external I/O in the modules already responsible for them. Connect dependencies where the project already does so. Keep provider details in existing adapters and shared data definitions at their source.
3. Reuse existing interfaces, factories, and classes when useful. Add layers only when the change needs them.
4. Follow plan dependencies and required checks before starting the next phase. Check affected callers after each meaningful part of the change.
5. Preserve input validation and useful errors. Use a fallback only when it is accepted behavior. Ask about missing business rules instead of guessing defaults.
6. Avoid unrelated cleanup, formatting, and unneeded dependencies. Explain non-obvious decisions in comments. Removing existing functionality needs permission and an explanation of the impact.

For refactors or interface changes, check public imports, function arguments, API fields, errors, and callers. Preserve required event order, data source tracking, and stored-data behavior.

## Step 3: Verify the behavior

1. Use the project's environment and test commands. Start with focused checks; run broader checks when the change affects more of the system.
2. Check the requested behavior, important failure cases, and regressions. For a bug, show the test fails before the fix and passes after it when practical.
3. When a change crosses modules, check the affected callers and the actual product flow.
4. For async or stored state, check the affected timing, cancellation, shutdown, concurrent updates, restart, or migration behavior.
5. Record commands and passed, failed, or skipped results. Separate existing failures from new failures. Report missing services, models, files, or environment setup.
6. Keep required checks outstanding if they cannot run. Mocks, imports, and syntax checks do not prove live service behavior or model quality.

If a check fails, investigate the cause. Do not weaken assertions or add patches that hide the failure.

## Step 4: Review and finish

1. Review the actual diff against the request. Use `code-reviewer` for the review the task needs. Provide the requirements, diff, affected interfaces, and check results.
2. Verify findings before fixing them. Apply valid fixes within the approved scope and rerun affected checks. Label self-review accurately; follow the project's independent-review requirements.
3. Update directly affected existing docs with `architecture-docs`. Include changed paths, commands, environment setup, and public behavior. Do not create unrelated guidance.
4. Update an existing approved plan with verified progress. Mark it `done` only after its tasks are implemented and verified. Do not create a plan just to mark it complete.

Report what changed, checks and results, review status, remaining gaps, and compatibility or data-safety impact. Keep implementation, product checks, review, and release status separate.
