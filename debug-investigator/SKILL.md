---
name: debug-investigator
description: Systematically investigate the root cause of a bug, test failure, or unexpected behavior before any fix.
argument-hint: Bug description, failing test, error message, or unexpected behavior.
---

Use this skill the moment a bug, test failure, perf issue, or unexpected behavior appears. Investigation only — do not change code yet. Hand off to `thoughtful-coder` once root cause is confirmed.

## Iron Law

**No fixes without root-cause investigation first.** Symptom patches waste time and mask deeper bugs. Phase 1 must finish before any fix proposal — including "obvious" ones.

## When to use

Bug reports, failing tests, prod incidents, perf regressions, build failures, integration breaks. Especially when:
- Time pressure pushes toward a quick patch.
- An "obvious" fix is tempting.
- Previous fixes didn't stick.

## Phase 1 — Root cause investigation

1. Read the error/failure message **carefully**. Capture exact stack trace and inputs.
2. Reproduce reliably. If you can't reproduce, you can't fix.
3. `codegraph_callers` on the failing function → who triggers this path?
4. `codegraph_impact` on the suspect symbol → what else uses it?
5. Review recent changes touching this path (`git log -p <file>`, blame the line).
6. Trace data flow **backward** from failure point to source; instrument boundaries with logs if state isn't clear.

Output of Phase 1: a one-sentence root cause stated as cause→effect, not symptom.

## Phase 2 — Pattern analysis

Find working examples nearby:

- `codegraph_search` for similar functions/patterns that already work correctly.
- Open the working version, **read it fully**, then enumerate **every** difference vs the broken path — config, types, ordering, error handling, dependency versions. Small differences matter.

## Phase 3 — Hypothesis & testing

- State **one** specific hypothesis: "X causes Y because Z".
- Test with the **smallest possible change** (a print, a guard, a single value swap). Don't bundle changes.
- Verify the hypothesis against evidence before continuing. Admit uncertainty rather than guessing — failed hypotheses are useful data.

## Phase 4 — Hand off

Once root cause is confirmed:

1. Write a failing test that captures the bug.
2. Hand the root cause + test to `thoughtful-coder` for the surgical fix.
3. After the fix lands, re-run the full test suite (not just the new test) — verify nothing else broke.

## Red flags — stop and restart Phase 1

- "Quick fix for now, refactor later"
- Trying random changes hoping one sticks
- Bundling multiple edits in one attempt
- Skipping the failing test
- Proposing a fix before tracing data flow

## Circuit breaker

After **3 failed fix attempts**, stop. The architecture is wrong, not the line of code. Surface this to the user and re-scope.

## No-root-cause case

If the cause genuinely points to environment/timing (flaky network, race in external system), document findings and add **retries / timeouts / monitoring** at the boundary — but assume incomplete analysis first.
