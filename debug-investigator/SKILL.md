---
name: debug-investigator
description: Systematically investigate the root cause of a bug, test failure, or unexpected behavior before any fix.
argument-hint: Bug description, failing test, error message, or unexpected behavior.
---

Use this skill the moment a bug, test failure, perf issue, or unexpected behavior appears. Investigation only — do not change code yet. Hand off to `thoughtful-coder` once root cause is confirmed.

## Iron Law

**No fixes without root-cause investigation first.** Symptom patches waste time and mask deeper bugs — this applies even under time pressure, even when a fix looks "obvious", and especially when previous fixes didn't stick. The investigation is done when you can state the root cause in one sentence as cause→effect, backed by evidence — not before.

## Investigation

The goal is a root cause you can defend with evidence, not a plausible story. What counts as evidence: a reliable reproduction, the exact error and inputs, the data flow traced from failure point back to source, and recent changes to that path ruled in or out. If you can't reproduce it, you can't claim to have fixed it.

Choose your own tools and order — codegraph for callers/impact, git history, logs, instrumentation, whatever the bug calls for. Two practices are worth keeping regardless of approach:

- **Compare against working code.** A nearby function or path that does the same thing correctly is the fastest diff for spotting the broken assumption — config, types, ordering, error handling. Small differences matter.
- **Test one hypothesis at a time.** State it as "X causes Y because Z" and probe it with the smallest possible change (a print, a guard, a single value swap). Don't bundle changes — a failed hypothesis is useful data only if you know which change failed. Admit uncertainty rather than guessing.

## Hand off

Once root cause is confirmed:

1. Write a failing test that captures the bug.
2. Hand the root cause + test to `thoughtful-coder` for the surgical fix.
3. After the fix lands, re-run the full test suite (not just the new test) — verify nothing else broke.

## Red flags — stop and restart the investigation

- "Quick fix for now, refactor later"
- Trying random changes hoping one sticks
- Bundling multiple edits in one attempt
- Skipping the failing test
- Proposing a fix before tracing data flow

## Circuit breaker

After **3 failed fix attempts**, stop. The architecture is wrong, not the line of code. Surface this to the user and re-scope.

## No-root-cause case

If the cause genuinely points to environment/timing (flaky network, race in external system), document findings and add **retries / timeouts / monitoring** at the boundary — but assume incomplete analysis first.
