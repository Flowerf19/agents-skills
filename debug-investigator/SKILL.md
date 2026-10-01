---
name: debug-investigator
description: Find why a failure happens before proposing a fix. Use when the user asks to diagnose or debug a bug, failing test, build error, slow behavior, or unexpected result, or when an approved fix needs investigation. Diagnosis alone does not give permission to edit code.
argument-hint: Symptom, failed test, error, logs, or reproduction steps.
---

# Debug investigator

Explain what failed, why it failed, and where the smallest fix belongs.

Keep the repository unchanged for diagnosis-only requests. Use existing tests, read-only inspection, or isolated temporary checks. Do not change production data or expose secrets. Add a tracked reproduction test only when the task allows it. An approved end-to-end fix can continue to coding after the cause is confirmed.

## Step 1: Capture the failure

1. Record expected and actual behavior, inputs, errors, and environment.
2. Check the source branch or commit, project guides, and how the failing flow starts.
3. Try the smallest reproduction. State whether the failure was reproduced.
4. Check current behavior against accepted requirements. Do not assume a mocked flow or old document describes the real failure.

## Step 2: Trace the cause

1. Follow data and calls through the affected modules to the returned result.
2. Find the first point where behavior differs from the requirement.
3. Compare recent changes and a working flow when useful. Do not copy unrelated business rules from it.
4. For async or stored state, inspect relevant event order, cancellation, concurrent updates, restart, and logs. A success response alone does not prove the operation finished.

## Step 3: Test an explanation

1. State a possible cause and the observation that would disprove it.
2. Run the smallest check that separates it from other possible causes.
3. Record what the result confirms or rules out.
4. Revise the explanation when evidence contradicts it. Do not make random edits or label a guess as confirmed.

For intermittent failures, collect evidence where the failure occurs. Do not replace diagnosis with blanket retries or larger timeouts.

If checks stop producing useful evidence, stop the investigation. Report what was ruled out and which observation or access is missing. Do not declare the design wrong after an arbitrary number of attempts.

## Step 4: Return the diagnosis

Report:

- Expected and actual behavior, with reproduction steps when available.
- Confirmed cause and evidence: file locations, logs, or check results.
- Unconfirmed explanations separately.
- Affected callers or data, and the smallest place to fix the problem.
- A regression check and any broader checks the fix will need.
- Whether implementation is approved.

If a design question blocks the fix, pass the evidence to `implementation-planner`. If the cause is confirmed and coding is approved, continue with `thoughtful-coder`. Otherwise stop after the diagnosis. Review the resulting code diff with `code-reviewer`; a diagnosis alone is not a code review.
