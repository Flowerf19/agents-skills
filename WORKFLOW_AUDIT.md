# Workflow source audit

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
