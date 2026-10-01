# Agent Skills

Shared rules and skills for coding agents. [AGENTS.md](AGENTS.md) sets the common permissions and project checks. Each skill adds steps for one kind of task.

## Core workflow skills

| Skill | Use for | Result |
|---|---|---|
| [implementation-planner](implementation-planner/SKILL.md) | Compare designs or plan a change | Recommendation or approved plan file; no code edits |
| [debug-investigator](debug-investigator/SKILL.md) | Diagnose a bug, failed test, build error, or slow behavior | Cause, evidence, and proposed fix; diagnosis alone does not allow edits |
| [thoughtful-coder](thoughtful-coder/SKILL.md) | Implement an approved feature, fix, or refactor | Focused change and check results |
| [code-reviewer](code-reviewer/SKILL.md) | Review a diff, PR, or local changes | Findings and verdict; no edits |
| [architecture-docs](architecture-docs/SKILL.md) | Write or update project guidance and prompts | Approved document changes; no runtime edits |

Use only the skills the task needs. A small, clear change does not need a formal plan. An end-to-end fix includes the needed investigation, implementation, checks, review, and related docs within its approved scope.

## Work from the current project

1. Read the request, project instructions, affected code, callers, and tests. Check the branch or commit and existing changes.
2. Check relevant project guides. A `.agents/` guide may need to be opened manually. Separate current code, accepted requirements, and old document claims.
3. Plan when needed. Identify affected modules, inputs and outputs, task dependencies, and required checks. Task numbers do not define execution order.
4. Make the approved change using the project's existing modules and interfaces.
5. Check the changed behavior, its callers, and the relevant product flow. Report passed, failed, and skipped checks. Missing required checks remain outstanding.
6. Review the actual diff, apply valid fixes within scope, and update directly affected docs. Commit, merge, and release need their own permission.

Discussion-only, diagnosis-only, and review-only requests stop at their requested result. Do not run the full workflow for every task.

These rules come from recurring patterns in the owner's projects. Keep workflow control separate from external calls, shared data definitions at their source, and tests connected to affected callers. Use each project's own database, agent stages, and UI conventions.

[WORKFLOW_AUDIT.md](WORKFLOW_AUDIT.md) records the source evidence and prompt reviews. It is review history, not a file every task must read.

## Installation and host configuration

Personal installation:

```bash
git clone https://github.com/Flowerf19/agents-skills.git ~/.claude/skills
```

Use an unused destination; do not overwrite an existing installation or configuration. For a project-pinned installation, add the repository as a submodule:

```bash
git submodule add https://github.com/Flowerf19/agents-skills.git .agents/skills
```

Configure skill discovery for the host. Claude Code discovers personal skills under `~/.claude/skills/` and project skills under `.claude/skills/`. Pi also discovers `~/.agents/skills/` and project `.agents/skills/`; a symlink can expose the same installation without copying files.

Load the shared [AGENTS.md](AGENTS.md) through the host's recognized instruction entrypoint, separately from skill discovery. For example, reference it from Claude Code's personal `~/.claude/CLAUDE.md`, or use it as Pi's `~/.pi/agent/AGENTS.md`. Preserve existing host instructions when configuring this. Installing a skill does not automatically load this repository's common guide in every host.

CodeGraph and other MCP services are optional, task-specific tools. An existing `.agents/` documentation tree is not a prerequisite. Verify the active provider and exact model ID before spawning subagents; do not assume a model name or map an unspecified tier.

## Invocation

Claude Code supports natural-language selection and explicit commands:

```text
Compare architecture options for rate limiting. Do not edit files.
/implementation-planner Plan rate limiting for the gateway; do not implement.
/code-reviewer HEAD~1..HEAD
```

Pi's explicit command uses a different prefix:

```text
/skill:implementation-planner Compare rate-limiting approaches; do not edit files.
/skill:code-reviewer Review HEAD~1..HEAD without modifying files.
```

Other hosts may use different discovery and invocation conventions; consult their documentation rather than assuming Claude Code syntax.

Each `SKILL.md` includes `name`, `description`, and an optional `argument-hint`. Descriptions state what the skill does, when to select it, and relevant task keywords. Body instructions are loaded when needed. In Claude Code, arguments without a receiving placeholder are appended to the skill content; Pi appends explicit skill arguments as a user request. This suite does not require `$ARGUMENTS` substitution. `argument-hint` is a Claude Code extension; API or other portable packaging may require removing unsupported frontmatter fields.

## Writing and permissions

Write shared rules and skills in plain English. State the goal and allowed changes first. Use short sentences and numbered steps when order matters. Keep details such as migration, concurrency, and benchmarks conditional on the task.

Keep common rules in `AGENTS.md` and task-specific steps in the skill. Each skill still states its own permission limit. Do not add repeated explanations of standard programming practices.

A recommendation, an accepted design, and permission to implement are different. Plan status records progress; it does not approve work. Do not invent business rules to fill missing requirements.

Split code for useful responsibilities, not a fixed line count. Do not create documentation folders, fill word quotas, or stop debugging after a fixed number of attempts just because a skill is active.

Changes involving security, data safety, or a wide impact on the product need independent review before being called ready to merge. Report missing review as outstanding. Self-review is not independent review.

## Verification

Check YAML metadata, description limits, local links, and the actual diff. Then test both natural-language selection and explicit invocation with arguments in the target host. Include negative scenarios:

- An architecture discussion must not produce unauthorized code or a self-approved plan.
- A review-only request must not modify files.
- Missing classification requirements must not produce fabricated keyword rules.
- An authorized end-to-end fix must not ask for redundant approval at every handoff.
- Pre-existing changes must remain separate; commit and push require authorization.

Static checks and independent prompt review do not establish runtime compliance. Inspect actual tool calls and repository changes during these tests, not just the agent's explanation.

## Sources

- [Anthropic Prompt Library](https://code.claude.com/docs/en/prompt-library): outcome-oriented requests, concrete references, verification criteria, and explicit task boundaries.
- [Claude Code Best Practices](https://code.claude.com/docs/en/best-practices): separate exploration and planning from implementation, keep persistent guidance concise, and constrain reviews to meaningful requirements.
- [Skill Authoring Best Practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices): descriptions should explain both what a skill does and when to use it, with specific terms and contexts.
- [Claude Code Skills](https://code.claude.com/docs/en/skills): discovery, invocation, frontmatter, argument handling, and packaging differences.
- [Pi Skills](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/skills.md): Pi discovery and `/skill:name` invocation.

These sources inform the prompting and discovery patterns. The suite's authorization gates and risk-based review requirements are repository policy, not claims that Anthropic mandates this exact workflow.
