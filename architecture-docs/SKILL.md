---
name: architecture-docs
description: Create and maintain concise project architecture and agent guidance docs based on real repository evidence.
argument-hint: Project path, architecture-docs goal, or docs to sync.
---

Use this skill to create or refresh `.agents/` guidance docs from current repo evidence. Documentation-only — do not edit runtime code, tests, configs, Docker, or deps. If fixing docs reveals a code issue, mention it as a follow-up.

## Audit before write

Before refreshing or rewriting docs, scan for staleness — drift between docs and current code is the most common doc bug:

1. List all markdown under `.agents/` and root-level docs (`README*.md`, `CLAUDE.md`).
2. Extract every `file/path.ext`, symbol name, env var, and command mentioned.
3. Cross-check against reality:
   - File path → `ls` / `codegraph_files`
   - Symbol → `codegraph_search`
   - Env var → grep `.env.example` / `Config` class
   - Command → run `--help` or verify the script exists
4. Flag stale references and contradictions across files (same fact, different values).

Fix stale references as part of the same task; surface contradictions in the output report.

## Evidence workflow

1. Read instruction entrypoints first: `AGENTS.md`, `CLAUDE.md`, `.github/copilot-instructions.md`, `.agents/README.md`.
2. Read only docs targeted by the requested change.
3. Use codegraph for structural facts (components, entrypoints, symbols, call flow, impact). Do not re-derive these in the docs themselves — codegraph already answers them on demand.
4. Inspect manifests/runtime files only to verify commands, env vars, services, deps.
5. When framework/API behavior isn't verifiable from the repo, query Context7 MCP first; fall back to web search only if Context7 lacks it.

## Default `.agents/` layout

- `README.md` — index, read order, codegraph policy, critical boundaries.
- `PROJECT_CONTEXT.md` — runtime modes, architecture boundaries, memory tiers, key env vars.
- `AGENT_RULES.md` — safety, workflow, style, PR hygiene, verified gotchas.
- `TESTING_GUIDE.md` — test layout, commands, service dependencies.

Extra subfolders allowed when a real recurring need exists:
- `plans/` — implementation plans (active or recently finished, with `status:` field).
- `decisions/` — ADR-style records of accepted architectural decisions.
- `runbooks/` — operational procedures triggered repeatedly (incident response, release).

Trigger threshold: ≥3 files of the same shape, or an operation repeated on a schedule. Do not pre-create empty subfolders.

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
