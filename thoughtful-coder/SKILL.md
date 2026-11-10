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

1. Make the smallest complete change, reusing working project patterns. For an approved replacement, follow the clean-cutover rule in AGENTS.md: move the affected flow fully to the new design rather than preserve the old path to keep the diff small.
2. Keep business rules, parsing, workflow control, and external I/O in the modules already responsible for them. Connect dependencies where the project already does so. Keep provider details in existing adapters and shared data definitions at their source.
3. Reuse existing interfaces, factories, and classes when useful. Add layers only when the change needs them.
4. Check affected callers after each meaningful part of the change. For an approved replacement, update all affected in-project callers, imports, exports, tests, configuration, and docs in the same task, then remove superseded code and dead branches. Keep only compatibility exceptions explicitly required under AGENTS.md. Preserve required event order, data source tracking, and stored-data behavior.
5. Preserve input validation and useful errors. Use a fallback only when it is accepted behavior, not to hide an incomplete cutover. Ask about missing business rules instead of guessing defaults. If a required external contract or an unapproved data migration or destructive action blocks the replacement, report the conflict and ask; do not silently add compatibility code or delete data.
6. Avoid unrelated cleanup, formatting, and unneeded dependencies. Removing superseded implementation within an approved replacement is required cleanup, not permission to remove unrelated or still-required functionality. Other functionality removals need permission and an explanation of the impact.
7. Add a short explanatory comment above a function or method only when its behavior, intent, rule, or limit is not clear from the code. Follow the language-specific comment syntax and docstring placement rules in AGENTS.md. Do not add comments that merely repeat self-explanatory code.
8. Follow the project's design style. Where the code is object-oriented, give each class one clear concept. Keep its data and the work on that data together, hide internal state behind methods, and prefer composition over deep inheritance. Do not force classes onto plain scripts or data-only modules that the project keeps simple.
9. Keep one clear responsibility per file. When a file passes 500 lines, check what it holds. If it holds two or more responsibilities, split it along that boundary when the task scope allows, or report the split as follow-up work. A longer file with one clear responsibility may stay; say why in the report.

## Step 3: Verify the behavior

1. Use the project's environment and test commands. Start with focused checks; run broader checks when the change affects more of the system.
2. Check the requested behavior, important failure cases, and regressions. For a bug, show the test fails before the fix and passes after it when practical.
3. When a change crosses modules, check the affected callers and the actual product flow. For a replacement, verify that superseded paths are gone except for explicitly required compatibility, and that the new path covers the accepted behavior and important failures. Replace obsolete test expectations only when the accepted contract changed; do not weaken checks for still-required behavior. Check that no affected in-project callers remain on the replaced interface.
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
