---
name: architecture-docs
description: Write and update project guidance using verified code and accepted decisions. Use when the user asks to write, audit, or simplify architecture docs, README.md, AGENTS.md, SKILL.md, or .agents guidance, or when an approved change needs related docs. Documentation only; do not edit runtime code.
argument-hint: Document request, prompt rewrite, or approved change with affected docs.
---

# Architecture docs

Write guidance that is easy to follow and matches accepted decisions and verified facts.

Edit only requested documents or docs directly affected by an approved implementation. Do not edit code, tests, configuration, or dependencies to make a document true. Do not change plan approvals or task status unless the request includes them or an approved implementation has verified progress to record.

## Step 1: Read and check the facts

1. Read the request, project instructions, accepted decisions, and documents in scope.
2. Follow relevant links in the existing project guides. Do not assume every project has the same documentation folders.
3. Verify file paths, symbols, commands, and behavior against current source. Record the branch or commit when comparing projects.
4. Separate current behavior, accepted work that is not implemented, and proposals. Report disagreements that affect the document.

## Step 2: Choose where to write

1. Update the existing document that owns the guidance.
2. Change only the files needed for consistency. Link to shared rules instead of repeating them.
3. For a new requested document, use the project's layout or a simple path for its purpose. Do not create a whole documentation tree.

Keep a root README focused on purpose, requirements, usage, checks, and important limits. Add a diagram only when it explains a verified design.

## Step 3: Write clear instructions

1. Use plain English for shared instructions and skills. Use the requested language for other documents. Keep identifiers and commands exact.
2. State the goal and scope first. Use short sentences with a clear action.
3. Use numbered steps when order matters. Put optional or task-specific details under a clear condition.
4. Keep project rules the agent cannot infer from the code. Remove repeated advice, stale paths, unsupported claims, and arbitrary size limits.
5. Preserve security and data-loss warnings in full.

For skills, check when each skill is selected, what it may change, what it returns, and where it stops. A handoff cannot grant extra permission.

When learning from other projects, use working code and tests as evidence. Reuse useful design principles; keep project-specific databases, agent stages, business rules, and themes out of shared requirements.

## Step 4: Review and report

1. Check links, paths, commands, names, versions, and contradictions across the changed documents.
2. For a prompt rewrite, walk through representative requests and check permissions, steps, outputs, and handoffs. Use independent review when required and available.
3. Run safe checks needed to support factual claims. Do not describe a proposed flow as tested.
4. Update affected existing project guides or module READMEs when the approved work changes their instructions.

Report changed documents, check results, and unresolved questions. Keep document errors separate from confirmed runtime bugs.

Send a runtime bug to `debug-investigator`, a design question to `implementation-planner`, or an approved code correction to `thoughtful-coder`. Do not perform those code changes during documentation-only work.
