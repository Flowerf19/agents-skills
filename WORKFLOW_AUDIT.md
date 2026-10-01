# Workflow audit and prompt review

The source audit below records the work merged in [PR #1](https://github.com/Flowerf19/agents-skills/pull/1). It is historical evidence, not required context for every task.

## Source audit baseline

Audit date: 2026-10-01. Target baseline: [`f1e72be`](https://github.com/Flowerf19/agents-skills/commit/f1e72be8debcf8655f047f2c0e017ac6a17cf3e1). Scope: common rules and the five coding-workflow skills, compared with selected source, contracts, tests, and project guidance on the source repositories' `main` snapshots below. This is a workflow audit, not a comprehensive bug audit of those projects. Other branches, unpushed work, and production deployments were not inspected.

## Assessment

The latest revision has the right foundation: separate authorization from recommendations and review, avoid invented domain rules, preserve ownership, and require evidence. Keep those changes. The remaining mismatch is operational specificity: discovering project context, reconciling current code with accepted intent, planning contract dependencies, and proving the changed boundary through its consumers.

One concrete suite contradiction was found. The other findings below are workflow coverage gaps demonstrated by the projects, not evidence that an agent has already failed in a live run.

| Finding at the baseline | Evidence and consequence | Draft correction |
|---|---|---|
| **Important: skill discovery assumes one personal path.** `AGENTS.md` requires `~/.claude/skills/`, while README also offers a project submodule and other hosts. | A project-pinned installation can satisfy README setup without existing at the path the common guide requires. | Resolve the active host/project-selected installation; retain the personal path as an option. |
| **Important: project-context discovery is implicit.** The common rules say to read applicable instructions but do not explicitly follow project guide/context/testing files. | Several projects keep decisive runtime boundaries and execution gates under `.agents/`, which is not guaranteed to be loaded by the host. | Discover existing project guides and follow their relevant references; do not generate a mandatory documentation scaffold. |
| **Important: stale guidance versus accepted target lacks a shared decision procedure.** | Thyca's `.agents/README.md:13` says `0.8.5.dev0`; its package manifest says `0.86.5.dev0`. Another Brain's guide calls Plan 07 `in-progress`, but that plan's frontmatter says `done`. The reranker guide says metrics/runner are absent even though their source exists. Blindly following such status text can produce duplicate or obsolete work. | Separate actual behavior, accepted target, and historical/status claims; expose material drift. Source does not authorize changing an accepted requirement. |
| **Important: handoffs omit explicit owner/consumer and gate records.** The planner includes dependencies, but the shared lifecycle does not carry them through implementation and review. | Another Brain explicitly executes GOAL-015 before numerically earlier goals. Thyca module plans list consuming modules and retained import surfaces. A numeric task walk or local-only refactor can miss a prerequisite or break another owner. | Carry owners, contracts, invariants, dependency-ready tasks, affected callers, and phase gates through the working packet. |
| **Important: verification is risk-aware but evidence layers are underspecified.** | Another Brain's installed-product/restart tests can skip without its model or console script. Reranker mock sanity checks do not demonstrate real model quality. ModelsReview preserves invalidated measurements. A green partial suite cannot substantiate those omitted claims. | Distinguish isolated, integration, and product/acceptance evidence; report skipped gates and model/service/artifact prerequisites explicitly. |

## Source evidence

These are pinned observations, not a claim that every existing implementation is ideal or has been executed in this audit.

| Repository / `main` snapshot | Source inspected | Transferable convention |
|---|---|---|
| [thyca-ai / dc1c997](https://github.com/Flowerf19/thyca-ai/tree/dc1c9977bfcc687fa4b2cecf2c5829bcf1cba7f5) | `thyca/agent/{act,think}.py`, `thyca/app/toolchain.py`, `tests/test_agent_act.py`, module M1 and backend audit plans, project rules | Runtime stages operate through `ToolDispatcher`/`LLMPort`; concrete wiring has a composition point. Module plans preserve consumer/import contracts. Do not universalize the four agent phases. |
| [another-brain / b88f0c9](https://github.com/Flowerf19/another-brain/tree/b88f0c9f46179b028667d8b8da918d36717d5a99) | `another_brain/protocols.py`, `services/memory_service.py`, `retrieval/service.py`, Plan 07, testing guide, installed stdio/cutover tests | Service dependencies use explicit contracts; lexical/vector ownership is separate. Execute dependency gates rather than task-number order. Installed-product, model, persistence/restart, and migration evidence have distinct prerequisites. Do not generalize its storage or domain constants. |
| [March7 / 08afd04](https://github.com/Flowerf19/March7/tree/08afd0481201628a237e1830cdd652546c222603) | `gateway/core/handler.py`, `gateway/shared/handler_base.py`, `services/system_gateway/approval_ledger.py`, `tests/unit/agent_loop_test.py`, project rules and gateway removal plan | Platform adapters stay outside core orchestration; ordering and prerequisite-tool behavior are observable contracts. Durable single-use authorization needs state/concurrency verification. Product authorization is separate from a coding assistant's scope. |
| [RAG / c65986b](https://github.com/Flowerf19/RAG/tree/c65986b32f9fe3b575ec6776bd09dd08ba892427) | `pipeline/rag_pipeline.py`, `pipeline/processing/{pdf_processor,embedding_processor}.py`, `chunkers/base_chunker.py`, `embedders/{i_embedder,embedder_factory}.py`, chunk provenance model | A pipeline coordinates specialized processing/storage modules with contracts and factories. Preserve provenance across stages. Do not copy legacy import workarounds, approximate token rules, or defaults merely because they exist. |
| [vietnamese-reranker-benchmark / 1ef0480](https://github.com/Flowerf19/vietnamese-reranker-benchmark/tree/1ef048081b9b167a9e5c40f2b5ec680e5bd4a324) | `src/rerankbench/adapters/base.py`, `benchmark/runner.py`, `scripts/sanity_adapters.py`, project guide and benchmark plan | The runner uses one scoring contract; model-family details belong inside adapters. Fixed comparison conditions and rank-based metrics are domain-specific acceptance criteria. Source, mock results, and actual benchmark evidence are different. |
| [ModelsReview / ef32ec3](https://github.com/Flowerf19/ModelsReview/tree/ef32ec3ca472ad6417bfd402da3f214b315c6bc7) | `scripts/bench.py`, `scripts/aggregate.py`, README and comparison guidance | Measurements capture runtime/output metadata and retain invalidated runs separately. Configuration and evidence validity matter; smoke success does not prove model quality. Hardware-specific paths are not a shared convention. |
| [my_health_v001 / aa0de67](https://github.com/Flowerf19/my_health_v001/tree/aa0de673c234740287ec3f30f10d05cd5b3e5f2a) | Auth provider/service, chat message model/service, `lib/` inventory | UI state, feature services, and data models are distinct owners even outside Python. Do not prescribe Python Protocols or the same layout to Flutter. Concrete construction and broad catches here are observations, not patterns automatically endorsed for new code. |
| [finance-agent-vn / d915de7](https://github.com/Flowerf19/finance-agent-vn/tree/d915de7bfdb487fcb8fc0ea4fffe74d1f09cbc16) | `src/agent/agent_core.py`, `src/api/vnstock_tools.py`, corresponding tests, Copilot guide | The guide expresses an orchestration/integration separation, but source/tests are placeholder headers. Treat it as design intent, not working runtime evidence. |

An additional private document-processing benchmark was inspected through its shared client, scoring utility, and backend wrapper. Its source and data are not reproduced here; the shared conclusions above have public source support. Repository inventories for the ASR benchmark and LLM cookbook contained guidance/plans but no corresponding runtime source, so they were not used as implementation evidence.

## Resulting workflow

Use the project's current layout and vocabulary. For non-trivial authorized work:

1. Establish source ref, existing changes, entrypoint, project context, accepted contract, and reproducible current behavior.
2. Plan only as needed: identify ownership, input/output and invariants, consumers, dependency order, and verification gates. Keep consequential unresolved decisions open.
3. Implement a dependency-ready slice at the existing ownership/composition point. Preserve interfaces and product behavior required by consumers.
4. Verify isolated behavior, the affected caller/adapter seam, and the relevant product path. Keep skipped or unavailable acceptance evidence outstanding.
5. Review the actual combined diff, validate findings, correct within scope, and rerun affected checks. Use independent review when required and available.
6. Synchronize affected existing guidance and record accurate completion, acceptance, and review status. Commit, merge, and release retain separate authorization.

Small, understood changes need no formal plan. Diagnosis-only, architecture-only, and review-only requests stop at the requested product. An end-to-end request does not need repeated permission at ordinary handoffs. These steps do not require spawning agents, creating classes, using all five skills, or introducing a universal directory scaffold.

## Verification and limits

The draft changes common rules, README, and the five workflow skills. Optional design/presentation/library skills and the source projects remain unchanged.

Checks: `git diff --check`; YAML metadata on the five changed skills; their local links and README links; preserved authorization boundaries; source-project clean working trees; and a manual scenario walkthrough listed below. These are static/self-review checks. No source project's runtime suite, model benchmark, natural-language skill discovery, target-host invocation, or deployment was run. Independent behavioral review remains outstanding: this session does not expose a verified active model/provider identity satisfying the repository's subagent rule.

| Walkthrough scenario | Expected instruction path | Static assessment |
|---|---|---|
| Architecture comparison only | Establish context → planner recommendation; no code or self-approval | Consistent |
| Review-only request | Establish actual diff/contract → reviewer findings; no edits/status mutation | Consistent |
| Authorized end-to-end bug fix | Reproduce → dependency-ready fix → tests → review/docs within scope | Consistent; no repeated approval required |
| Missing semantic classification contract | Expose uncertainty and seek the consequential decision | Consistent; no invented keyword defaults |
| Refactor a consumed module | Identify canonical owner and public surfaces → integration check with callers | Explicitly covered |
| Numbered goals with non-numeric dependencies | Follow explicit dependencies and phase gates | Explicitly covered |
| Stale project status versus current source | Distinguish runtime facts from accepted intent; report material drift | Explicitly covered |
| Acceptance test skips without a model/artifact | Report passed/failed/skipped separately; leave the gate outstanding | Explicitly covered |
| Project-pinned skills on another host | Resolve active host/project installation instead of one personal path | Contradiction corrected |
| Flutter/UI task using shared guidance | Respect feature/page/shared owners and current design; do not impose Python layers | Explicitly covered |
| Existing unrelated working-tree edits | Keep them separate throughout edits and close-out | Preserved |
| Request to pause, or no commit permission | Preserve current work and stop the restricted action | Preserved |

Runtime forward-tests are still needed before claiming reliable skill activation or agent compliance. The source drift noted above is evidence for improving discovery, not authorization to edit those projects during this task.


## Plain-English rewrite and sequential self-review

Date: 2026-10-01. Rewrite baseline: [`687bbb4`](https://github.com/Flowerf19/agents-skills/commit/687bbb449bdd6704c73cb28b8bcecaa1a3cb0e7f), after PR #1 was merged.

The user requested simple language, separate steps where needed, and a review after each prompt. The editing order was common rules, planner, coder, reviewer, debug, then docs. Each file was compared with its baseline before proceeding. README was then updated to match the prompts. The source-audit evidence above was retained.

Review method: read the diff, check for unclear actions or lost requirements, correct confirmed gaps, then inspect the corrected instructions. These were same-context self-reviews, not independent reviews or runtime tests.

| Prompt | Rewrite | Review and corrections | Words before / after |
|---|---|---|---|
| `AGENTS.md` | Plain common rules; project reading steps; shorter skill routing | Kept separate permissions for implementation and commit/merge, existing edits, missing business rules, host-selected skills, and independent review. Clarified that existing edits must not be included without permission. | 1380 / 944 |
| `implementation-planner` | Understand, resolve questions, write the needed plan, check and hand off | Restored explicit flow tracing, failure cases, and missing-check consequences. Kept dependency order, conditional details, plan history, and status permissions. | 691 / 577 |
| `thoughtful-coder` | Understand, change, verify, review and finish | Clarified that a bug needs cause evidence, provider details stay in adapters, and shared definitions stay at their source. Kept caller checks, conditional state checks, and skipped-check reporting. | 839 / 650 |
| `code-reviewer` | Set scope, check relevant parts, validate findings, return the review | Kept read-only scope, severity meanings, and attempts to disprove findings. Restored explicit heuristic counterexamples and comparison of reported results with evidence. | 679 / 630 |
| `debug-investigator` | Capture failure, trace cause, test an explanation, return diagnosis | Kept diagnosis-only work unchanged, safe temporary checks, cause versus guess, useful stopping conditions, and coding only within approved scope. No further issue confirmed in this static pass. | 487 / 474 |
| `architecture-docs` | Check facts, choose location, write clearly, review and report | Kept document-only edits, plan-status limits, and full security/data-loss warnings. Clarified requested language for documents other than shared English instructions. | 646 / 538 |

Word counts use whitespace-separated tokens including frontmatter and headings. Total: 4722 to 3813, a 19.3% reduction. This measures length only; it does not measure model understanding.

### Combined scenario walkthrough

The 12 source-audit scenarios above were checked again against the rewritten prompts. The following cases received extra attention:

| Request or condition | Instruction path checked | Static result |
|---|---|---|
| Compare designs; do not edit files | Planner boundary and handoff | Conversation recommendation; no code or unsolicited plan file |
| Review a PR; a real bug is found | Reviewer boundary and final step | Findings only; no automatic fix or plan-status edit |
| Fix a reproduced bug end to end | Debug handoff, coder steps, common permission rules | Continue within scope without repeated approval at ordinary handoffs |
| A small change with no design question | Common routing and planner step 1 | No formal plan or documentation scaffold required |
| Refactor a module used by another module | Planner conditional details, coder steps 2–3, reviewer integration check | Preserve required interfaces and check callers |
| A required model is unavailable | Planner prerequisites, coder check results, reviewer required checks | Report the missing check; do not treat mocks as live proof |
| A prompt rewrite finds a runtime defect | Docs boundary and final handoff | Report or hand off the defect; do not edit runtime code |
| A new business rule or architecture is needed | Common permission rules and planner questions | Keep the choice open and ask before implementing it |
| Runtime model/provider identity is unavailable | Common subagent rule | Do not spawn; label same-context review as self-review |

### Checks and remaining work

Static checks: `git diff --check`, five YAML headers and their description limits, four ordered steps in each skill, local Markdown links, and comparison with the baseline permissions and workflow requirements. Optional skills and source projects were not edited.

No confirmed issue remained in the static self-review after the listed corrections. This is not a merge-readiness certification. Target-host skill selection, explicit invocation, actual tool-call behavior, and independent review remain unverified. No Claude API evaluation or source-project runtime suite was run.

Writing references checked during the discussion:

- [Claude prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices): clear actions, needed context, and ordered steps when order matters.
- [Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices): concise instructions, useful descriptions, conditional detail, and task-appropriate freedom.
- [Claude Code best practices](https://code.claude.com/docs/en/best-practices): short, human-readable common guidance and removal of unnecessary instructions.
