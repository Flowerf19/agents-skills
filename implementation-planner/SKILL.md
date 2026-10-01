---
name: implementation-planner
description: Explore architecture options and create evidence-grounded implementation plans. Use when the user asks to plan a feature, compare design approaches, decide architecture, or turn a spec into an execution plan, or when an authorized non-trivial change needs planning. Planning does not authorize implementation.
argument-hint: Feature, architecture question, bug handoff, or existing plan.
---

# Implementation planner

## Outcome and boundary

Produce a recommendation or an execution-ready plan that connects the user's goal to repository evidence and a verifiable result.

Planning does not authorize implementation. For an architecture discussion, answer in the conversation; do not create a plan file unless the user requests a written artifact or has authorized an implementation task that needs one. Do not edit runtime code, tests, configuration, dependencies, or unrelated guidance.

## Ground the decision

- Read the current request, applicable project instructions, existing decisions or plans, and the affected flow. Use actual code and tests rather than an assumed file inventory.
- Establish the source branch/ref and runtime entrypoint. Follow the project's guide into its rules, context, decisions, and testing commands; distinguish stale status, implemented behavior, accepted target contracts, and historical plans. A draft plan may contain accepted decisions without authorizing execution.
- Trace the producer and consumer across the affected modules. Identify current ownership, canonical types/schema, dependency direction, and composition points before proposing a layout. Borrow responsibility boundaries from similar working code, not a fixed class count or another project's runtime pipeline.
- State the desired outcome, existing limitation, constraints, and what is out of scope.
- For an unresolved design, compare viable options and their correctness, compatibility, cost, and verification trade-offs. Recommend one without treating it as accepted.
- Ask targeted questions when the answer changes correctness or scope. Unapproved rules, mappings, thresholds, or architecture remain open decisions; do not encode them as executable defaults.
- If a bug is not understood, use `debug-investigator` for cause-and-effect evidence before planning a fix. If a small requested change is already clear, skip formal planning.

## Plan artifact

Use an existing plan and repository format where available. Otherwise write an authorized artifact to `.agents/plans/<slug>.md`; create only the directory needed for that artifact, not a documentation scaffold.

```yaml
---
status: draft
created: YYYY-MM-DD
last_updated: YYYY-MM-DD
---
```

Include only what another implementer needs:

- Goal, acceptance criteria, authorized scope, and non-goals.
- Accepted decisions with their source; unresolved decisions separately.
- Small tasks tied to outcomes, relevant ownership points, and dependencies. Name interfaces or files when they remove execution ambiguity, not to prescribe every tool call.
- For a boundary change, identify input/output contracts, invariants, state transitions, error/cancellation behavior, and affected callers. For a refactor, state the public behavior/import/wire/data surfaces that must remain compatible. For UI work, include affected routes, page/shared ownership, and existing design tokens when relevant.
- State dependency order and phase gates explicitly. Keep a blocked task blocked; do not infer execution order from GOAL/TASK numbering. If parallel work is supported and useful, assign non-overlapping ownership and identify the shared-contract integration check.
- Failure cases, compatibility and data-safety concerns, and any approved migration or rollback needs.
- Runnable verification commands and expected behavior. For performance or classification, use agreed metrics and representative cases, not invented thresholds.
- Separate isolated tests, caller/adapter integration, and the relevant product-path or acceptance gate. State the required interpreter, service/model/artifact prerequisites, and what a skipped gate leaves unverified. Keep mock, measured, and live evidence distinct.

Preserve existing task IDs and completed history. Update `last_updated` whenever the plan changes. Record genuine changes of scope instead of silently rewriting what was approved. Do not present a plan with unresolved blocking decisions as execution-ready.

## Handoff and lifecycle

Return the recommendation or artifact path, evidence supporting it, open decisions, and whether implementation is authorized. Pass accepted requirements, boundaries, and verification criteria to `thoughtful-coder` only when coding is authorized; otherwise stop after planning.

A new proposal is `draft`. Use `in-progress` only when execution is authorized and underway. The implementer records `done` after the authorized tasks are implemented and verified; commit and merge are separate permissions. Use `abandoned` with a reason when superseded, and preserve the artifact unless its deletion is authorized.
