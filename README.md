# Agent Skills

Shared working rules and task-specific skills for coding agents. [AGENTS.md](AGENTS.md) defines permission boundaries, evidence standards, and skill routing. Each skill defines its outcome, scope, verification, and handoff without granting new authority.

## Core workflow skills

| Skill | Use when | Result and boundary |
|-------|----------|---------------------|
| [implementation-planner](implementation-planner/SKILL.md) | Comparing architecture options, planning a feature, or turning a spec into an implementation plan | Evidence-grounded recommendation or authorized plan artifact; no implementation or self-approval. |
| [debug-investigator](debug-investigator/SKILL.md) | Diagnosing a bug, failing test, build error, or performance regression | Cause-and-effect evidence and a correction point; investigation alone does not authorize a fix. |
| [thoughtful-coder](thoughtful-coder/SKILL.md) | Implementing an explicitly requested feature, fix, refactor, or implementation plan | Scoped diff and behavior verification; unresolved consequential decisions go back to the user. |
| [code-reviewer](code-reviewer/SKILL.md) | Reviewing a diff, pull request, or uncommitted changes | Evidence-backed findings and a verdict; review is read-only and does not authorize merge. |
| [architecture-docs](architecture-docs/SKILL.md) | Writing, auditing, or synchronizing architecture and agent guidance | Authorized document changes based on verified reality; no runtime changes or mandatory scaffolding. |

These skills are a toolkit, not a mandatory chain. Small, understood implementation requests need no formal plan. An end-to-end fix can include investigation, implementation, verification, review, and directly related documentation within the authorized scope.

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

## Why this instruction-suite revision exists

The revision makes three distinctions explicit: a recommendation is not an accepted design, an accepted design is not permission to implement, and a skill handoff cannot expand the user's authorization. A plan remains `draft` until execution is authorized; its status records progress rather than granting approval. `done` means the authorized work is implemented and verified, not that an unrequested commit or merge occurred. Plan changes update `last_updated`.

It also prevents missing domain requirements from being filled with invented keyword classifiers, thresholds, mappings, or fallback labels. Regex remains appropriate for verified syntax. Semantic heuristics need an identifiable basis, representative cases, counterexamples, and error evaluation; deterministic code and LLM output are not substitutes for a domain contract.

This revisits [commit d70202a](https://github.com/Flowerf19/agents-skills/commit/d70202a6b0627e6eb58a0acbda8e7721541cacbf), which introduced SOLID separation and a 300-line class cap. Responsibility boundaries remain important, but a fixed line count is no longer a reason to split code or demand a refactor. Mandatory documentation bootstrapping, README word quotas, and a fixed debugging-attempt cutoff were also removed because they can cause unrelated work rather than prove correctness.

Reviewers now substantiate and try to refute findings instead of reporting every uncertain concern as a defect. Security-sensitive, data-safety, and high-blast-radius changes still require independent review before being reported ready to merge; unavailable review stays outstanding. Self-review is not independent review.

Repeated references to a personal absolute AGENTS.md path were removed from the five skill bodies. Shared policy stays in the host-loaded common guide, while each skill retains its own permission boundary. This reduces duplication and external path coupling; it does not certify API portability or reliable auto-trigger behavior.

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
