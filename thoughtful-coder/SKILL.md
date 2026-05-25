---
name: thoughtful-coder
description: Careful, surgical code changes — minimal diff, reuse existing patterns, no speculative abstraction.
argument-hint: A coding task, bug, refactor, or feature to implement carefully.
---

Use this skill to make code changes. Optimise in this order: Correctness → Minimal diff → Consistency with repo → Verifiable outcome → Simplicity.

## Before editing

- If `.agents/` is missing, run the `architecture-docs` skill first.
- Read instruction entrypoints only: `AGENTS.md`, `.agents/README.md`, `.github/copilot-instructions.md`.
- Use codegraph for structure (definitions, callers/callees, impact, feature context). Read source only after codegraph identifies the target file.
- Verify runtime constraints from manifests: language version, deps, Docker, env vars, framework conventions.
- For external API/library tasks, check official docs.

## Core rules

- Every changed line supports the request. No drive-by fixes — mention unrelated issues separately.
- Match existing style and patterns; reuse existing helpers; clean unused imports your change introduces.
- Do not rename unrelated symbols, reformat unrelated files, or add formatters/linters/deps without an explicit ask.
- Add focused tests when behavior changes.
- Ask only when ambiguity affects correctness; otherwise pick a safe default and state it.

## Flow

Multi-step work: reproduce/identify → smallest fix → verify. Convert vague requests into verifiable outcomes (tests, logs, health checks, reproducible commands).

## Doc handoff

After a non-trivial change, append to your response:

```md
## Documentation impact
- README impact: yes/no
- Agent guidance impact: yes/no
- Architecture/runtime impact: yes/no
- Testing guide impact: yes/no
- Suggested follow-up: `create-readme`, `architecture-docs`, or none
- Reason: short explanation
```

Do not rewrite broad docs in the same coding task — defer to the relevant skill.

## Response shape

Non-trivial: Understanding & assumptions → Approach → Implementation → Verification → Notes.
Trivial: short summary + verification.

## Final check

Diff is minimal, change solves the request, repo patterns respected, verification specific, unresolved risks stated.
