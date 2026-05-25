---
name: architecture-docs
description: Create and maintain concise project architecture and agent guidance docs based on real repository evidence.
argument-hint: Project path, architecture-docs goal, or docs to sync.
---

Use this skill to create or refresh `.agents/` guidance docs from current repo evidence. Documentation-only — do not edit runtime code, tests, configs, Docker, or deps. If fixing docs reveals a code issue, mention it as a follow-up.

## Evidence workflow

1. Read instruction entrypoints first: `AGENTS.md`, `.github/copilot-instructions.md`, `.agents/README.md`.
2. Read only docs targeted by the requested change.
3. Use codegraph for structural facts (components, entrypoints, symbols, call flow, impact). Do not re-derive these in the docs themselves — codegraph already answers them on demand.
4. Inspect manifests/runtime files only to verify commands, env vars, services, deps.
5. Use official external docs only when framework/API behavior isn't verifiable from the repo.

## Default `.agents/` layout

- `README.md` — index, read order, codegraph policy, critical boundaries.
- `PROJECT_CONTEXT.md` — runtime modes, architecture boundaries, memory tiers, key env vars.
- `AGENT_RULES.md` — safety, workflow, style, PR hygiene, verified gotchas.
- `TESTING_GUIDE.md` — test layout, commands, service dependencies.

Do not create extra per-style/git/glossary/per-task files unless the repo has a repeated operational need that won't fit above.

## What belongs in docs

- Exact commands and paths.
- Runtime/service ownership boundaries.
- Security constraints and known high-risk tools.
- Env var names and verified defaults when useful.
- Testing and verification guidance.
- Decisions or gotchas not obvious from code.

## What to skip (codegraph territory)

- Symbol inventories, file structure dumps, caller/callee chains, class hierarchies.
- Speculative future architecture, marketing language, TODO/TBD, duplicated explanations across files.

## Output

Report docs changed, evidence used, key decisions, known gaps, suggested follow-ups. Concise.
