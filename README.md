# Agent Skills

Shared rules and skills for coding agents. [AGENTS.md](AGENTS.md) is the canonical rule set — permissions, tool tiers, project grounding, and skill routing — and the host loads it in every session. Each skill adds steps for one kind of task and loads only when the task matches. This README covers installation and verification for humans; it does not repeat the rules in AGENTS.md.

## Core workflow skills

| Skill | Use for | Result |
|---|---|---|
| [implementation-planner](implementation-planner/SKILL.md) | Compare designs or plan a change | Recommendation or approved plan file; no code edits |
| [debug-investigator](debug-investigator/SKILL.md) | Diagnose a bug, failed test, build error, or slow behavior | Cause, evidence, and proposed fix; diagnosis alone does not allow edits |
| [thoughtful-coder](thoughtful-coder/SKILL.md) | Implement an approved feature, fix, or refactor | Focused change and check results |
| [code-reviewer](code-reviewer/SKILL.md) | Review a diff, PR, or local changes | Findings and verdict; no edits |
| [architecture-docs](architecture-docs/SKILL.md) | Write or update project guidance and prompts | Approved document changes; no runtime edits |

[WORKFLOW_AUDIT.md](WORKFLOW_AUDIT.md) records the source evidence and prompt reviews behind this suite. It is review history, not a file every task must read.

## How loading works

- Load AGENTS.md through the host's recognized instruction entrypoint, separately from skill discovery. For example, reference it from Claude Code's personal `~/.claude/CLAUDE.md`, or use it as Pi's `~/.pi/agent/AGENTS.md`. Preserve existing host instructions when configuring this. Installing the skills does not automatically load AGENTS.md in every host.
- The host advertises each skill's name and description; the SKILL.md body loads when a task matches or on explicit invocation.
- Some subagent runtimes disable skill discovery while still loading AGENTS.md. The routing table in AGENTS.md names each skill and the standard install directories, so those agents can open the needed SKILL.md directly.

## Installation

Personal installation:

```bash
git clone https://github.com/Flowerf19/agents-skills.git ~/.claude/skills
```

Use an unused destination; do not overwrite an existing installation or configuration. For a project-pinned installation, add the repository as a submodule:

```bash
git submodule add https://github.com/Flowerf19/agents-skills.git .agents/skills
```

Configure skill discovery for the host. Claude Code discovers personal skills under `~/.claude/skills/` and project skills under `.claude/skills/`. Pi also discovers `~/.agents/skills/` and project `.agents/skills/`; a symlink can expose the same installation without copying files.

CodeGraph and other MCP services are optional, task-specific tools. An existing `.agents/` documentation tree is not a prerequisite.

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

Each `SKILL.md` has `name`, `description`, and an optional `argument-hint`. Descriptions state what the skill does, when to select it, and relevant task keywords. This suite does not require `$ARGUMENTS` substitution. `argument-hint` is a Claude Code extension; API or other portable packaging may require removing unsupported frontmatter fields.

## Verification

Check YAML metadata, description limits, local links, and the actual diff. Then test both natural-language selection and explicit invocation with arguments in the target host. Include negative scenarios:

- An architecture discussion must not produce unauthorized code or a self-approved plan.
- A review-only request must not modify files.
- Missing classification requirements must not produce fabricated keyword rules.
- An authorized end-to-end fix must not ask for redundant approval at every handoff.
- Pre-existing changes must remain separate; commit and push require authorization.
- With skill discovery disabled, an agent that still loads AGENTS.md must route to the right SKILL.md from the table there.

Static checks and independent prompt review do not establish runtime compliance. Inspect actual tool calls and repository changes during these tests, not just the agent's explanation.

## Sources

- [Anthropic Prompt Library](https://code.claude.com/docs/en/prompt-library): outcome-oriented requests, concrete references, verification criteria, and explicit task boundaries.
- [Claude Code Best Practices](https://code.claude.com/docs/en/best-practices): separate exploration and planning from implementation, keep persistent guidance concise, and constrain reviews to meaningful requirements.
- [Skill Authoring Best Practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices): descriptions should explain both what a skill does and when to use it, with specific terms and contexts.
- [Claude Code Skills](https://code.claude.com/docs/en/skills): discovery, invocation, frontmatter, argument handling, and packaging differences.
- [Pi Skills](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/skills.md): Pi discovery and `/skill:name` invocation.

These sources inform the prompting and discovery patterns. The suite's authorization gates and risk-based review requirements are repository policy, not claims that Anthropic mandates this exact workflow.
