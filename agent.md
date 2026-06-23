# Agent guidance

Shared, host-agnostic operating instructions for any coding agent working in these projects. This file lives in the skills repo so every host (regardless of vendor) reads the same source of truth. Each host's native config file (e.g. `CLAUDE.md`, `AGENTS.md`, `GEMINI.md`) should only point here.

## Skills

Curated skills live at `~/.claude/skills/` (clone of [Flowerf19/agents-skills](https://github.com/Flowerf19/agents-skills)) — apply to every project, every agent.

- `implementation-planner` — turn spec/feature/bug into execution-ready plan (BEFORE writing code).
- `thoughtful-coder` — surgical code changes: Correctness → Minimal diff → Consistency → Verifiable → Simplicity.
- `debug-investigator` — root-cause investigation BEFORE any fix. Iron law: no patch until cause is identified.
- `code-reviewer` — independent review of a change after `thoughtful-coder` completes; before merge.
- `architecture-docs` — maintain/refresh `.agents/` docs after architectural changes.
- `create-readme` — write/update root README from real repo evidence.

Hosts with native skill discovery load these automatically (e.g. via a Skill tool or `/<skill-name>`). Hosts without it should read `~/.claude/skills/<name>/SKILL.md` directly.

Update upstream: `cd ~/.claude/skills && git pull`.

## Handling feedback (from user or reviewer)

When you receive feedback:

- **No performative agreement.** Banned: "You're absolutely right!", "Great point!", "Good catch!". They're filler that signals compliance, not understanding.
- **Verify before implementing.** Read the actual code, run the test, check the assumption. Feedback can be wrong about this codebase even when it's correct in general.
- **Ask if unclear.** Don't half-implement. Items may be interconnected.
- **Push back when wrong** — with evidence (file:line, test output, codegraph result), not defensiveness. State the disagreement factually.
- **One issue at a time.** Don't bundle fixes; each item gets its own diff so you can revert cleanly.
- **Just fix it** when feedback is correct. Describe what changed in one sentence. No long apology if you were wrong earlier — state the correction and move on.

## Orchestration

For non-trivial tasks, act as a coordinator: plan and delegate to specialized subagents (`implementation-planner`, `thoughtful-coder`, `debug-investigator`, `code-reviewer`, `architecture-docs`, `create-readme`) rather than doing everything in one context. Manage context across them and merge their outputs. Keep each subagent's job narrow; chain them in sequence (plan → code → review) instead of overloading a single context.

## Subagents

When you delegate to a subagent:

- **Pass a skill**: Name the relevant skill in the subagent's prompt so it follows the right playbook — e.g. "Use `debug-investigator` to find the root cause", "Use `thoughtful-coder` for the fix", "Use `code-reviewer` to review the diff". The subagent loads it from `~/.claude/skills/<name>/SKILL.md`. Match the skill to the job, not the other way around.
- **Self-assess the model — never hardcode a skill→model map**: Judge each dispatch on reasoning depth, ambiguity, blast radius (how much breaks if the output is wrong), and breadth of context needed. Then pick the *cheapest model that can do it reliably* — deep reasoning / architecture / root-cause / security-sensitive review lean to your strongest model; well-scoped work against a clear plan sits mid-tier; mechanical, templated, lookup, or formatting work goes to the lightest/fastest model. When genuinely unsure, inherit the session model instead of guessing.
- **Mechanism**: use your host's model-override for the dispatch (e.g. an Agent tool's `model` param, or `agent(prompt, {model, agentType})` in a workflow). The *same* skill may run under different models on different dispatches — the model follows the task, not the skill.

## MCP

Reach for MCP tools proactively for execution quality and context integrity:

1. **Codegraph**: For any structural query — class/function definitions, symbol searches, caller/callee queries, impact analysis — run CodeGraph MCP tools (`codegraph_search`, `codegraph_context`, `codegraph_callers`, etc.) first. Never manually grep or read through files for structural analysis if CodeGraph can query it.
2. **Context7**: Whenever the user asks about, or you implement code with, external libraries, SDKs, or frameworks (React, Next.js, Prisma, Kafka, etc.), query Context7 via `resolve-library-id` then `query-docs` for the latest docs, APIs, and patterns. Fall back to web search only if Context7 doesn't have the library.

## CodeGraph

In repositories indexed by CodeGraph (a `.codegraph/` directory exists at the repo root), reach for it BEFORE grep/find or reading files when you need to understand or locate code:

- **MCP tools** (when available): `codegraph_explore` answers most code questions in one call — the relevant symbols' verbatim source plus the call paths between them. `codegraph_node` returns one symbol's source + callers, or reads a whole file with line numbers. If the tools are listed but deferred, load them by name via tool search.
- **Shell** (always works): `codegraph explore "<symbol names or question>"` and `codegraph node <symbol-or-file>` print the same output.

If there is no `.codegraph/` directory, skip CodeGraph entirely — indexing is the user's decision.
