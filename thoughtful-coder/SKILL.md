---
name: thoughtful-coder
description: Make approved code changes and check that they work. Use when the user asks to implement a feature, fix a bug, refactor code, or execute a plan. Do not use for discussion, planning, or review-only requests.
argument-hint: Code change, approved plan, or confirmed bug fix.
---

# Thoughtful coder

Deliver the requested behavior, a focused diff, and evidence from relevant checks.

Edit only when implementation is approved. An accepted design or review suggestion alone is not permission. Complete the approved work; ask before a new decision changes its scope.

## Step 1: Before editing

1. Keep changes that were already present separate from this task.
2. For a bug, reproduce it and find its cause, or verify the diagnosis evidence. Resolve a blocking design question before coding around it.
3. Choose how to verify the requested behavior before editing.

## Step 2: Make the change

1. Make the smallest change that fully solves the task, reusing working project patterns.
2. Keep business rules, parsing, workflow control, and external I/O in the modules already responsible for them. Connect dependencies where the project already does so. Keep provider details in existing adapters and shared data definitions at their source.
3. Reuse existing interfaces, factories, and classes when useful. Add layers only when the change needs them.
4. Check affected callers after each meaningful part of the change. For refactors or interface changes, check public imports, function arguments, API fields, errors, and callers. Preserve required event order, data source tracking, and stored-data behavior.
5. Preserve input validation and useful errors. Use a fallback only when it is accepted behavior. Ask about missing business rules instead of guessing defaults.
6. Avoid unrelated cleanup, formatting, and unneeded dependencies. Removing existing functionality needs permission and an explanation of the impact.
7. Put a short comment directly above every method or function you add or change. Use the comment syntax of the language (`#` in Python, `//` in TypeScript or Go). Write one line in plain words: what the method does, and why it exists when the name does not say it. Word it by the "Plain writing" rules in AGENTS.md. A one-line docstring in the same place counts when the file already uses docstrings for every method. Follow the same form for comments you add inside the code to explain a hard part.
8. Follow the project's design style. Where the code is object-oriented, give each class one clear concept. Keep its data and the work on that data together, hide internal state behind methods, and prefer composition over deep inheritance. Do not force classes onto plain scripts or data-only modules that the project keeps simple.
9. Keep one clear responsibility per file. When a file passes 500 lines, check what it holds. If it holds two or more responsibilities, split it along that boundary when the task scope allows, or report the split as follow-up work. A longer file with one clear responsibility may stay; say why in the report.

## Step 3: Verify the behavior

1. Use the project's environment and test commands. Start with focused checks; run broader checks when the change affects more of the system.
2. Check the requested behavior, important failure cases, and regressions. For a bug, show the test fails before the fix and passes after it when practical.
3. When a change crosses modules, check the affected callers and the actual product flow.
4. For async or stored state, check the affected timing, cancellation, shutdown, concurrent updates, restart, or migration behavior.
5. Record commands and passed, failed, or skipped results. Separate existing failures from new failures. Report missing services, models, files, or environment setup.
6. Keep required checks outstanding if they cannot run. Mocks, imports, and syntax checks do not prove live service behavior or model quality.

If a check fails, investigate the cause. Do not weaken assertions or add patches that hide the failure.

## Step 4: Review and finish

1. Review the actual diff against the request, and get the code review the task needs. Provide the requirements, diff, affected interfaces, and check results. Label self-review accurately.
2. Verify findings before fixing them. Apply valid fixes within the approved scope and rerun affected checks.
3. Update directly affected existing docs. Do not create unrelated guidance.
4. Update an existing approved plan with verified progress. Mark it `done` only after its tasks are implemented and verified. Do not create a plan just to mark it complete.

Report what changed, checks and results, review status, remaining gaps, and compatibility or data-safety impact. Write the report in plain language, following the "Plain writing" section in AGENTS.md. Keep implementation, product checks, review, and release status separate.
