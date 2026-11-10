# Agent Skills

Shared rules and skills for coding agents. [AGENTS.md](AGENTS.md) is the source of truth. This README is only for people who install or check this set.

## What is inside

- `AGENTS.md` — shared rules for every agent.
- Five skills:
  - `implementation-planner` — plan before coding.
  - `debug-investigator` — find the cause first.
  - `thoughtful-coder` — make the approved change.
  - `code-reviewer` — review only; no edits.
  - `architecture-docs` — update docs and prompts only.
- [WORKFLOW_AUDIT.md](WORKFLOW_AUDIT.md) — history only. Do not use it as the current workflow.

## Install

Personal install:

```bash
git clone https://github.com/Flowerf19/agents-skills.git ~/.claude/skills
```

Project install:

```bash
git submodule add https://github.com/Flowerf19/agents-skills.git .agents/skills
```

Point your host to `AGENTS.md`. Installing this repo alone does not load it.

Pi reads `~/.agents/skills/` and `.agents/skills/`. Claude Code reads `~/.claude/skills/` and `.claude/skills/`. A symlink is fine.

## Use

Write the task in normal language. The host picks the skill from its name and description.

Pi example:

```text
/skill:code-reviewer Review HEAD~1..HEAD. Do not edit files.
```

Claude Code example:

```text
/code-reviewer HEAD~1..HEAD
```

## Simple workflow

1. Clarify or plan when needed.
2. Do only approved work.
3. Run the needed checks.
4. On the multi-agent route, resume the same planning/review conversation after each delegated handoff. PLAN and REVIEW share that conversation; the coding agent keeps its own separate conversation for implementation and corrections. On the direct route, check the change against the request and record the results.
5. Report what passed, what failed, and what is still open.

## Quick checks

- A discussion must not turn into code changes.
- A review must not edit files.
- Every delegated code handoff needs `REVIEW` in the retained planning/review conversation, not a new reviewer.
- A direct-route task must not bypass a pending delegated review.
- A failed review is not a pass.

## Notes

A static read of a skill is not a measured check. A measured check needs realistic cases with expected behavior, including cases that should trigger the skill and near-miss cases that should not. Compare against a baseline in fresh sessions. Keep the actual output, trace, and timing as evidence. This repo has no such measurements yet.

MCP servers belong to host config, not this repo. On this machine, Pi gets CodeGraph and Context7 from `pi-config`.

## License

MIT — see [LICENSE](LICENSE).
