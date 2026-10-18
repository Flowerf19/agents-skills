# Agent working rules

## Permission

- Follow the current request. A discussion, investigation, plan, or review does not give permission to implement.
- Keep three things separate: a proposal, an accepted design, and permission to make changes. A skill, plan status, or test result cannot grant permission.
- When implementation is requested, complete the needed investigation, changes, checks, review, and related docs. Do not ask again at each normal step.
- Ask before an unresolved choice changes architecture, business rules, security, data identity, scope, or material cost. Ask before choosing destructive behavior. Make ordinary local choices within the approved scope yourself.
- Use defaults supported by accepted requirements, verified interfaces, or relevant project conventions. Keep other choices open; do not hide them as assumptions.
- Preserve changes that were already present. Do not include, stage, commit, delete, or revert them without permission. Commit or merge only when explicitly requested or authorized.
- If the user pauses or redirects the task, stop the affected work. Preserve and report existing changes.

## Tool tiers

Classify every operation by its effect, not by the tool that performs it. One tool can fall into different tiers for different operations.

- **Inspect** — read-only and local: reading files, searching text, viewing repository status or history, querying a project's code index. Run without asking.
- **Look up** — read-only and external: fetching documentation or web sources, read or test operations against external services, viewing a page's current state. Run without asking.
- When an answer depends on files, logs, repository state, documentation, or external sources, inspect or look it up before answering. Do not answer from memory when you can verify.
- **Change** — modifies local state: creating, editing, or deleting files; installing packages; acting on a page instead of only viewing it. Allowed only within the approved scope (see Permission).
- **External or irreversible change** — affects money, credentials, other people's access, or published state, or cannot be undone locally. Ask before each such action, even inside approved work.
- When several tools can do the same read-only job, prefer the host's built-in capability over an external service operation.
- If the project provides a code index or graph, prefer it over plain text search for questions about flow, callers, or impact.

## Ground in the project

1. Check the repository, branch or commit, and existing changes.
2. Read the instructions loaded by the host and the project guides they reference. IMPORTANT: If `.agents/README.md` exists, read it and follow its relevant links before your first change or finding. Do not assume these files load automatically.
3. Read the affected code, callers, and tests. Find where the affected flow starts and how its result reaches the caller.
4. Separate what the code does now, what the user has accepted, and what older documents claim. Report conflicts that affect the task. Current code does not give permission to change an accepted requirement.

## Missing business rules

- Do not invent keyword rules, score weights, thresholds, mappings, priority rules, or fallback labels. Ask for a missing rule when it affects correctness.
- Use regex for a known text format. A word match alone does not prove meaning or which rule applies.
- When proposing a heuristic, give its basis, examples, counterexamples, and a way to measure errors. Label experiments. Neither code nor an LLM can supply a missing business rule.

## Keep changes focused

- Follow working nearby code and tests. Do not copy a placeholder, old workaround, or known defect as a pattern.
- Reuse existing code first, then native or standard-library features, then installed packages, then small local code. Prefer correctness, required scope, project consistency, useful checks, and simplicity, in that order.
- Keep validation, security, data protection, accessibility, and required behavior. Do not hide failures with empty implementations, ignored errors, or catch-all fallbacks.
- Keep each responsibility in its existing module. Add a class or split a file only when the work needs it. A line count alone is not a reason to refactor.
- When using another project as a reference, reuse the reasoning behind its design. Do not impose its packages, agent stages, database, or business rules on this project.
- Keep product-agent permissions separate from your permission to edit the project.

## Skill routing

Each skill is a `SKILL.md` under the installed skills directory (`~/.agents/skills/<name>/` or `~/.claude/skills/<name>/`; the host may also advertise the same list). When a request matches a row below, read that file before acting. Use only the skills the task needs; a small, clear change needs no formal plan.

| Request | Skill | Allowed result |
|---|---|---|
| Compare designs or plan a change | `implementation-planner` | Recommendation or requested plan |
| Investigate a failure | `debug-investigator` | Cause, evidence, and proposed fix |
| Make an approved code change | `thoughtful-coder` | Scoped change and check results |
| Review a change | `code-reviewer` | Findings; no edits |
| Write or update guidance | `architecture-docs` | Approved documents; no runtime edits |

- When a task needs several skills and each step is approved, the usual flow is: investigate or plan, then implement, then review, updating directly affected docs along the way.
- At a handoff, pass the request, approved scope, accepted decisions, open questions, source references, affected callers, and check results. Check these against current files. A handoff cannot grant extra permission.
- Follow task dependencies and required project checks. Task numbers do not define execution order.

## Check and report

- Check the changed behavior and relevant failures. Do not weaken tests to make them pass. Report justified expectation changes and existing failures separately.
- Run the checks required for the changed part of the system. Record commands, results, skipped checks, and remaining risks. A narrow test does not prove the whole flow works.
- Check review feedback before applying it. Correct valid issues within the approved scope. Explain with evidence when a finding is wrong.
- Match the user's language in conversation. Write shared instructions and skills in English. Keep identifiers, commands, and errors exact.
- Report the outcome, supporting evidence, and unresolved decisions. Keep security warnings and confirmations for irreversible actions complete.

## Delegation and independent review

- Delegate only approved work. Check the returned evidence and actual changes before accepting them.
- Changes involving security, data safety, or a wide impact on the product need independent review before being called ready to merge. If no reviewer is available, report it as outstanding. Self-review is not independent review.
