---
name: debug-investigator
description: Establish an evidence-backed root cause before a fix. Use when the user asks to diagnose, debug, or investigate a bug, failing test, build error, performance regression, or unexpected behavior, or when a requested fix needs root-cause analysis. Investigation alone does not authorize code changes.
argument-hint: Symptom, failing test, error output, logs, or reproduction.
---

# Debug investigator

## Outcome and boundary

Explain the failure with a cause-and-effect statement backed by a reproduction or direct evidence, and identify the smallest correction point.

Investigation does not authorize a fix. Keep the repository unchanged during a diagnosis-only request. Prefer existing tests, read-only inspection, or isolated temporary probes; do not mutate production data or expose secrets. Add a tracked reproduction test only when the authorized task includes it. An end-to-end bug-fix request may proceed to coding after cause confirmation without asking again.

## Investigate

- Capture expected behavior, actual behavior, exact inputs, error output, environment, and the affected boundary. Distinguish a reproducible defect from a report you cannot yet reproduce.
- Confirm the source ref, actual entrypoint, project context, and test/runtime environment. Separate current behavior from the accepted target and historical guidance; a mocked path or stale status may not describe the failing runtime.
- Trace the relevant data and control flow back to the first broken assumption. Check recent changes and compare with a working path where useful.
- Follow the contract across caller, orchestrator, adapter/storage, and returned result. Inspect the relevant ordering, cancellation, concurrent state, persistence/restart, and observability boundaries rather than assuming a success-shaped response proves completion. Use a neighboring working flow to isolate the difference without importing its unrelated policy.
- State a falsifiable hypothesis and use the smallest probe that can distinguish it from alternatives. Record what the result confirms or rules out.
- Revise the hypothesis when evidence contradicts it. Do not try random edits or call a plausible explanation a confirmed cause.
- For intermittent or environment-specific failures, collect evidence at the relevant boundary. Report uncertainty rather than prescribing blanket retries, timeouts, or monitoring as a substitute for a diagnosis.

If probes stop yielding discriminating evidence, stop, summarize what was ruled out, and identify the missing observation or access. Do not use an arbitrary attempt count to declare the architecture wrong or keep patching without a hypothesis.

## Handoff

Return:

- Expected versus actual behavior and a minimal reproduction, when available.
- Confirmed cause with file/line references, logs, or probe results; unresolved hypotheses separately.
- Affected callers or data and the smallest correction point.
- A regression check that detects the failure and the relevant broader verification.
- Whether a fix is authorized.

If correction requires an unresolved design, pass the evidence to `implementation-planner`. If the cause is confirmed and implementation is authorized, hand off to `thoughtful-coder`; otherwise stop with the diagnosis. The coder verifies the reproduction and regressions after the fix; `code-reviewer` evaluates the resulting diff, not the hypothesis alone.
