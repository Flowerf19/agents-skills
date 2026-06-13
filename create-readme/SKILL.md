---
name: create-readme
description: Generate concise, high-quality README files from real project structure, docs, manifests, and targeted code evidence.
argument-hint: Project path, README goal, and optional style constraints.
---

Use this skill to create or rewrite a project `README.md` from repository evidence. README-only — do not edit runtime code, tests, configs, deps, Docker, or agent docs (`.agents/`). If `.agents/` is missing or stale, mention it as a follow-up and prefer current repo evidence.

## Evidence priority

1. Valid, accessible project path.
2. Instruction entrypoints: `AGENTS.md`, `CLAUDE.md`, `.github/copilot-instructions.md`, `.agents/README.md`.
3. Existing README files.
4. Targeted project docs linked from the entrypoint (runtime, testing, provider, Docker, security).
5. Manifests + source structure: dependency files, Docker, `.env.example`, entrypoints, tests, scripts.
6. Codegraph for structural confirmation when needed.

Conflicts: prefer the most project-specific, recent, code-backed source. Surface conflicts only if they affect setup, architecture, usage, or safety.

## Codegraph policy

Use codegraph for structural questions (source layout, entrypoints, symbols, callers, callees). Do not dump structure into the README — codegraph already answers it on demand. Use native search/read only for literal text or files codegraph has already pinpointed.

If codegraph isn't available, fall back to targeted file search; do not do broad repository dumps.

## Workflow

1. Validate target path. Invalid: stop and ask.
2. Read instruction entrypoint, existing README, only docs relevant to the README goal.
3. Inspect manifests + commands: `package.json`, `pyproject.toml`, `requirements.txt`, Docker/compose, `.env.example`, Makefile/Taskfile, test config.
4. Write the README from evidence. Do not invent features, commands, architecture, or support promises.

When the README depends on external behavior the repo can't verify, query Context7 MCP first; fall back to web search only if Context7 lacks the library.

## Language

Match the user's request. If unspecified, match the dominant language of existing docs. Preserve identifiers, command names, env vars, and file paths exactly.

## README shape

GitHub Flavored Markdown, 500–700 words target, hard max 1000 unless the user asks for more.

Default sections:
1. Title + one-line value proposition
2. Key features
3. Architecture overview (Mermaid only when confirmed and it actually clarifies)
4. Project structure (high-level pointer, not exhaustive — codegraph handles detail)
5. Prerequisites
6. Quick start
7. Configuration
8. Development and testing
9. Troubleshooting / operational notes

GitHub admonitions sparingly for warnings/tips.

## Final response

Report the README changed, evidence used, important assumptions, conflicts or missing docs, verification performed. Concise.
