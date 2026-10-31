# Workflow audit

Last checked: 2026-10-31.

This is the short audit for the current prompt set. It replaces the long 2026-10-01 audit. Old source tables were removed on purpose. Use [AGENTS.md](AGENTS.md) for the rules that agents must follow now.

## Scope

Checked:

- `AGENTS.md`
- The five skills in this repo: `implementation-planner`, `debug-investigator`, `thoughtful-coder`, `code-reviewer`, and `architecture-docs`.
- The Pi fusion prompts in `pi-config`: `fusion-executor` and `fusion-planner-reviewer`.
- The check results written in the `pi-config` README.

Not checked as source code: the projects used only as test beds. Their results are test evidence, not rules to copy.

## Current shape

- `AGENTS.md` is the single source for shared rules.
- Skills give steps for one job. They point back to `AGENTS.md` instead of copying the same rule.
- On hosts with delegation, the lead does not edit code itself. Approved code, test, and config changes go to `fusion-executor`.
- Design and review use `fusion-planner-reviewer` in two fresh modes: `PLAN` and `REVIEW`.
- Every coding handoff needs a fresh `REVIEW`. Corrections need another fresh `REVIEW`.
- If `REVIEW` fails or gives no verdict, the lead starts one fresh `REVIEW`. If that also fails, the review stays open. The work is not called reviewed.
- Writing follows ISO 24495-1 plain language in `AGENTS.md`: short, clear, and usable on the first read.
- Code rules now cover method comments, simple object design, and one clear job per file. Passing 500 lines is a warning to check the file, not an automatic split rule.

## Tests

### Runtime tests

Results below are from real runs on 2026-10-31. The longer notes are in the `pi-config` README.

| Check | Result |
|---|---|
| Blind code-fix runs 2–4 | Passed. The lead delegated, checked the diff, and got `Approve` from `REVIEW`. Tests: 4/4. |
| Blind small-project tool run | Workflow passed. CodeGraph was not picked on a two-file toy project. This is expected at that size. |
| Blind tool run on `thyca-ai` | Passed. Without being told, the lead called `codegraph_explore` 8 times, then read small parts to confirm. |
| MCP smoke checks | Passed. Context7 and CodeGraph worked in Pi. Muse also called both tools after the fish shell got the key. |
| Creative HTML task | Passed by the project rule: built by `fusion-executor`, sent to `REVIEW`, fixes requested, fixes applied. |

### Static checks

Rechecked while writing this audit:

- Five skill frontmatter blocks: OK.
- Local links in `README.md` and this file: OK.
- `git diff --check`: OK.

### Scenario checks

Checked against the prompt text. These are instruction checks, not runtime proof:

- A discussion or plan request does not become code edits.
- A review request does not edit files.
- A missing business rule is asked about, not invented.
- A small, clear change skips formal planning.
- A failed review stays open; it is not called a pass.

Models used in those runs:

- Lead and `fusion-executor`: `meta/muse-spark-1.3-contributor`, `xhigh`.
- `fusion-planner-reviewer`: `openai-codex/gpt-6.1-sol`, `xhigh`.
- Pi default session model in settings: `meta/muse-wonder-1.0`.

## Issue found and fixed

One re-review could not finish because the reviewer hit a usage limit. The lead first tried to resume the failed review. That was the weak point.

`AGENTS.md` now says this in plain terms: do not resume a failed review. Start one fresh `REVIEW`. If it also fails, report the review as still open.

This fix is written in the prompt. It still needs a live re-test when the reviewer limit allows it.

## Still open

- Add the missing `clamp` boundary tests: value at both ends, and `minimum == maximum`.
- Re-test the failed-review path after the reviewer usage limit resets.
- Keep testing on real projects. Toy projects do not prove the workflow on large code.

## Update rule

Update this file only when a real run changes the picture: a new pass, a new failure, or a rule change in `AGENTS.md`, a skill, or a fusion prompt. Keep it short. Put long evidence in run logs or the `pi-config` README, not here.
