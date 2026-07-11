---
name: thoughtful-coder
description: Careful, surgical code changes — minimal diff, reuse existing patterns, no speculative abstraction.
argument-hint: A coding task, bug, refactor, or feature to implement carefully.
---

Use this skill to make code changes. Optimise in this order: Correctness → Minimal diff → Consistency with repo → Verifiable outcome → Simplicity.

## Before editing

- If `.agents/` is missing, run the `architecture-docs` skill first.
- Read instruction entrypoints only: `AGENTS.md`, `CLAUDE.md`, `.agents/README.md`, `.github/copilot-instructions.md`.
- Prefer codegraph for structural questions (definitions, callers/callees, impact, feature context) — it answers them cheaper than reading files. Read source once you know where to look.
- Verify runtime constraints from manifests: language version, deps, Docker, env vars, framework conventions.
- For external API/library tasks, query Context7 MCP first; fall back to web search only if Context7 lacks the library.

## Core rules

Each rule below guards a known LLM failure mode — silent assumptions, overengineering, drive-by edits — not a style preference. When a rule feels optional, it's usually one of these failing.

- Every changed line supports the request. No drive-by fixes — mention unrelated issues separately.
- Match existing style and patterns; reuse existing helpers; clean unused imports your change introduces.
- Do not rename unrelated symbols, reformat unrelated files, or add formatters/linters/deps without an explicit ask.
- Add focused tests when behavior changes.
- Ask only when ambiguity affects correctness; otherwise pick a safe default and state it.
- Comment dev-style: a single short comment line (`#`/`//` per language) directly above the one statement it explains — never a paragraph block narrating a whole section. Comment only lines that need it.

## Flow

Multi-step work: reproduce/identify → smallest fix → verify. Convert vague requests into verifiable outcomes (tests, logs, health checks, reproducible commands).

## Plan close-out

If the task came with an `implementation_plan.md`, tick each `TASK` you complete in its ledger (`Done` ✅ + date) and update its YAML header `status:` field as part of the change:

- `done` — implementation merged, plan kept as record (or moved to `.agents/decisions/` if it captures a long-lived architectural choice).
- `abandoned` — plan superseded; keep file with `status: abandoned` and a one-line reason.

Never leave a plan stuck in `in-progress` after the implementation lands.

## Doc handoff

After a non-trivial change, append to your response:

```md
## Documentation impact
- README impact: yes/no → `create-readme`
- Agent docs (`.agents/`) impact: yes/no → `architecture-docs`
- Plan close-out: <slug> → `done` / `abandoned` / N/A
- Reason: short explanation
```

Do not rewrite broad docs in the same coding task — defer to the relevant skill.

## Response shape

Lead with the outcome: what changed and whether it's verified, in the first sentence. Then supporting detail — approach, assumptions, notes — for readers who want it. Trivial changes: one-line summary + verification.

## Final check

Two litmus tests first: would a senior engineer call this overcomplicated, and does every changed line trace to the request? Then confirm: diff is minimal, change solves the request, repo patterns respected, verification specific, unresolved risks stated.
