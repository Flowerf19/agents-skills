---
name: architecture-docs
description: Write or synchronize architecture and agent guidance using verified repository evidence. Use when the user asks to write, update, audit, or consolidate architecture docs, README.md, AGENTS.md, SKILL.md, or .agents guidance, or when an authorized change requires documentation updates. Documentation only; do not edit runtime code.
argument-hint: Documentation request, instruction-file revision, or accepted change with affected docs.
---

# Architecture docs

## Outcome and boundary

Produce concise, consistent guidance that describes accepted decisions and verified reality.

Edit only documents authorized by the request or directly affected by an authorized implementation. Shared agent rules and skill files are valid targets when explicitly requested. Do not edit runtime code, tests, configuration, dependencies, or unrelated documents to make the documentation true. Do not bootstrap a documentation tree merely because another skill is being used.

## Ground and synchronize

- Read applicable instruction entrypoints, the current request, accepted decisions, and documents in scope. Verify paths, symbols, commands, and behavior against current repository evidence.
- Distinguish current implementation, approved but unimplemented design, and proposals. Label these states rather than merging them into a claim that a feature works.
- Choose the existing authoritative document or instruction file. Update the smallest set needed for consistency; link to a source of truth instead of duplicating it.
- For instruction suites, check the entry conditions, permissions, outputs, and handoffs together. A downstream skill must not grant itself authority denied by the common rules or the current request.
- Describe the outcome and constraints. Include exact procedures only where order matters for correctness, safety, or reproducibility; avoid prescribing tools and ceremony for ordinary work.
- Keep durable conventions and non-obvious constraints. Remove stale paths, conflicting rules, redundant generic advice, arbitrary size limits, unsupported claims, and speculative scaffolding. Preserve security and data-loss warnings in full.
- Match the language and structure requested for the artifact. Shared agent instructions and skills stay in English. Preserve exact identifiers and commands.

## Structure and verification

Use the repository's current layout. When a new document is authorized and no convention exists, select a minimal path for its actual purpose; do not generate README, context, rules, testing, plans, or runbooks as a mandatory bundle.

A root README should contain only the supported material needed by its audience: purpose, prerequisites, usage, verification, and important boundaries. Use diagrams only when they clarify a verified architecture; do not fill a word quota.

Before finishing, check referenced files, links, commands, terminology, version labels, and contradictions across the affected documents. Run safe checks when needed to substantiate behavior; do not claim a proposed flow was tested. For a shared instruction rewrite, check representative task scenarios for consistent permissions and handoffs, and obtain independent review when warranted.

## Handoff and output

Report documents changed, decisive consistency checks, and unresolved questions. Send a discovered runtime defect to `debug-investigator`, an unresolved architectural decision to `implementation-planner`, or an authorized implementation correction to `thoughtful-coder`; do not perform those changes as part of documentation-only work.

A documentation request does not authorize changing plan approvals or implementation status. Update a plan only when the requested documentation scope includes it or when recording verified progress from the authorized implementation.
