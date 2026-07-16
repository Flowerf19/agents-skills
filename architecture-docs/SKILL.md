---
name: architecture-docs
description: Create and maintain concise project architecture and agent guidance docs and root README files based on real repository evidence.
argument-hint: Project path, architecture-docs goal, or docs to sync.
---

Documentation-only: create or refresh `.agents/` guidance, architecture docs, and root `README.md`. Do not edit runtime code, tests, configs, Docker, or dependencies. Report code issues as follow-ups.

## Workflow

1. Read project instruction entrypoints and only the docs in scope.
2. Audit those docs for stale paths, symbols, env vars, commands, and contradictory claims.
3. Verify each claim against repo evidence using the shared tool rules in `~/.claude/skills/AGENTS.md`; run `--help` or tests when command behavior matters.
4. Update the smallest set of docs that restores consistency. Avoid copying the same fact into multiple files.

## Default `.agents/` layout

- `README.md` - index, read order, critical boundaries.
- `PROJECT_CONTEXT.md` - runtime modes, architecture boundaries, key env vars.
- `AGENT_RULES.md` - safety, workflow, style, verified gotchas.
- `TESTING_GUIDE.md` - test layout, commands, service dependencies.

Add subfolders only for a recurring need:

- `plans/` - implementation plans with lifecycle status.
- `decisions/` - accepted architectural decisions.
- `runbooks/` - repeated operational procedures.

Threshold: at least three files of the same shape, or a scheduled repeated operation. Never pre-create empty folders.

## Content boundary

Document exact commands and paths, ownership boundaries, security constraints, useful env vars, test guidance, and non-obvious decisions. Skip symbol inventories, exhaustive trees, call graphs, speculative architecture, marketing copy, TODO/TBD placeholders, and duplicated explanations.

## Root README

- Match the requested language; otherwise use the repo's dominant documentation language.
- Preserve identifiers, commands, env vars, and paths exactly.
- Target 500-700 words; hard max 1000 unless the user asks for more.
- Include only sections supported by repo evidence: purpose, key features, architecture overview, prerequisites, quick start, configuration, development/testing, troubleshooting.
- Use Mermaid only when the confirmed architecture is clearer with it.
- Do not invent features, commands, or support promises.

## Output

Report docs changed, decisive evidence, contradictions fixed, and remaining gaps.
