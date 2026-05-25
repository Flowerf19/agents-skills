---
name: code-reviewer
description: Independent review of a code change — verify it meets the requested behavior, with minimal diff, no security issues, no broken patterns, and adequate test coverage.
argument-hint: Git ref range, PR number, or description of the change to review.
---

Use this skill to get an independent second opinion on a code change after `thoughtful-coder` (or any agent) finishes implementation. Review only — do not edit code. Output: a categorised issue list the author acts on.

**Review early, review often.** Cheaper to fix at task boundaries than after merge.

## When to use

Mandatory:
- After each task in a multi-step plan.
- Before merging into the main branch.
- After a major feature lands.

Optional but valuable:
- When stuck (fresh perspective).
- Before refactoring (baseline check).
- After resolving a tricky bug.

## Prepare the context

```bash
BASE_SHA=$(git rev-parse HEAD~1)         # or origin/main
HEAD_SHA=$(git rev-parse HEAD)
git diff $BASE_SHA $HEAD_SHA             # the actual diff under review
```

Gather:
- The diff itself.
- The **requirement / plan** the change was supposed to implement (e.g. `implementation_plan.md`, issue, user spec).
- For impact analysis: `codegraph_impact` on changed symbols.

## How to review

Two modes:

1. **Inline** (default) — review in the current session. Lightweight, but the reviewer has seen the author's reasoning so independence is partial.
2. **Independent subagent** — spawn an Agent (e.g. `general-purpose` or `code-reviewer` agent if available) with the diff + plan only. Stronger independence; reviewer can't be biased by author thinking. Prefer this for high-stakes changes.

## What to check

| Dimension | Question |
|---|---|
| **Spec fit** | Does the diff actually deliver what the plan / issue asked for? |
| **Minimal diff** | Any drive-by changes, renames, reformats unrelated to the task? |
| **Correctness** | Edge cases, off-by-ones, null/empty inputs, async ordering, error paths. |
| **Security** | OWASP-style issues — injection, secret leakage, missing authz, unsafe deserialisation. |
| **Repo conventions** | Style, helpers reused, no speculative abstractions, import hygiene. |
| **Test coverage** | New behaviour covered? Failure modes tested? Tests actually assert something? |
| **Backward compat** | Public interfaces, env vars, schemas — breaking changes flagged? |

Use `codegraph_callers` on changed functions to confirm no downstream caller was silently broken.

## Output

Per issue:

```md
- **[Severity]** `path/file.ext:LINE` — short title
  - Problem: <one sentence>
  - Why it matters: <impact>
  - Suggested fix: <one line / code snippet>
```

Severities:
- **Critical** — security, data loss, broken core path. Block merge until fixed.
- **Important** — incorrect behaviour at the edges, missing tests for changed behaviour, broken convention. Fix before merge.
- **Minor** — style, naming, refactoring opportunity. Track, address when convenient.

End with one-line **verdict**: `Approve` / `Approve with fixes` / `Request changes`.

## After delivery

Author responds per `~/.claude/CLAUDE.md` feedback-handling rules: verify each item against the code, fix Critical/Important, track Minor. If reviewer is wrong, push back with evidence — don't agree performatively.

## Red flags during review

- Reviewer skips reading the plan and reviews diff in isolation.
- Approving without testing the behaviour or reading the test diff.
- Marking everything Minor to avoid friction.
- Suggesting refactors unrelated to the task (mention separately, not as review blockers).
