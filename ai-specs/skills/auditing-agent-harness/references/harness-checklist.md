# Specboot harness checklist (70 points)

Inspired by the [walkinglabs harness-engineering](https://walkinglabs.github.io/learn-harness-engineering/en/) curriculum. From here on this is the **Specboot audit** — score only against the Pass criteria below.

**Scoring:** Each row is 1 point. After discovering *this* repo’s paths, mark `pass` / `partial` / `fail` using **Pass**.  
**Headline score** = passes + 0.5×partials (or passes/70 plus a partial list).

**Severity:** `C` = critical for basic agent sessions · `R` = recommended depth.

---

## Subsystem 1 — Instructions (H01–H11)

| ID | Sev | Intent | Pass (discover locally) |
|---|---|---|---|
| H01 | C | Agent entry exists | A root agent entry exists (`AGENTS.md`, symlink to `docs/…`, or equivalent always-loaded standards). |
| H02 | C | System described early | Near the top of the router/standards, agents learn what the system is (product + stack in a few lines). |
| H03 | C | Verification discoverable | Agents can find how to verify work (test/lint/verify commands in router, runbook, or clearly linked docs). |
| H04 | R | Hard constraints stated | Standards state explicit must / must-not rules agents can follow without guessing. |
| H05 | R | State locations known | Standards tell agents where session state lives (OpenSpec change artifacts, `docs/`, `ai-specs/` — not chat memory). |
| H06 | R | Docs stay fresh | A rule or skill requires updating canonical docs in the same change as the code they describe. |
| H07 | R | Commit atomicity | Standards or commit skill: one logical change per commit; repo left consistent after each commit. |
| H08 | R | Entry is a router | Agent router is short and points outward; a long ops runbook (if any) stays separate. |
| H09 | R | Topic docs linked | Router links to topic standards / domain docs under `docs/` (or equivalent). |
| H10 | R | Constraints have why | Non-obvious rules include rationale (why / source / remove-when — any clear format). |
| H11 | R | Knowledge near code | Module or domain design notes exist where engineers look (co-located or clearly indexed from `docs/`). |

## Subsystem 2 — Tools (H12–H13)

| ID | Sev | Intent | Pass |
|---|---|---|---|
| H12 | R | Tool access scoped | Agent tool / MCP permissions are configured (settings, allowlists, or project MCP config). |
| H13 | R | Tools documented | Skills, agents, and permitted integrations are discoverable via `ai-specs/` and/or standards. |

## Subsystem 3 — Environment (H14–H18)

| ID | Sev | Intent | Pass |
|---|---|---|---|
| H14 | C | Reproducible installs | A dependency lockfile is committed for the primary package manager. |
| H15 | R | Runtime pinned | Exact runtime version is pinned (e.g. `.ruby-version`, `.nvmrc`, `.tool-versions`). |
| H16 | R | Task entry exists | There is a clear place for project tasks (`bin/*`, package scripts, Makefile, just, Taskfile, …). |
| H17 | R | One-command setup | Documented single command installs deps / prepares the app from scratch. |
| H18 | R | One-command dev | Documented single command starts the local dev server (or documented multi-process equivalent). |

## Subsystem 4 — State (H19–H21)

| ID | Sev | Intent | Pass |
|---|---|---|---|
| H19 | C | Cross-session progress | In-flight work has tracked progress in OpenSpec (`tasks.md` or equivalent), not only conversation history. |
| H20 | R | Decisions logged | Design rationale is written down (per-change `design.md` and/or lasting ADRs under `docs/`). |
| H21 | R | Work units listed | Work is broken into explicit units (`tasks.md` checkboxes + specs/scenarios). |

## Subsystem 5 — Feedback (H22–H24)

| ID | Sev | Intent | Pass |
|---|---|---|---|
| H22 | R | Full green pipeline named | Standards name the full “repo is green” combo (lint + tests ± other gates). |
| H23 | R | Tests runnable | A documented command runs the automated test suite. |
| H24 | C | Verify command documented | Same family as H03: verification is explicitly documented for agents (may overlap H03). |

## L05 — Cross-session continuity (H25–H30)

| ID | Sev | Intent | Pass |
|---|---|---|---|
| H25 | C | Current state snapshot | Status is visible from artifacts (tasks/reports/branch); idle repos with no active change may be `partial`. |
| H26 | R | Clock-in documented | Standards describe how to start: e.g. `openspec list`, read active `tasks.md`, confirm green baseline. |
| H27 | R | Clock-out documented | Standards describe how to end: update tasks/artifacts, re-check green, leave a clear handoff. |
| H28 | R | Don’t rush when context low | Explicit guidance: stop cleanly rather than rushing unfinished work when context is low. |
| H29 | R | Commit messages explain why | Commit guidance prefers why over “what only” (standards or commit skill). |
| H30 | R | Next actions explicit | Next work is obvious from unchecked tasks (or equivalent handoff list). |

## L03 — Repo as system of record (H31–H34)

| ID | Sev | Intent | Pass |
|---|---|---|---|
| H31 | R | Durability | Progress and decisions survive outside the agent context window (OpenSpec + `docs`). |
| H32 | R | Consistency predicate | A named green predicate exists (“repo consistent when … exits 0”). |
| H33 | R | Atomicity stated | Same family as H07 — atomic commits are stated as a rule. |
| H34 | R | Knowledge proximity | Same family as H11 — design knowledge is findable near the work. |

## L07 — WIP and completion ratio (H35–H36)

| ID | Sev | Intent | Pass |
|---|---|---|---|
| H35 | R | WIP discipline | One active OpenSpec change per worktree, or an explicit parallel-worktree policy. |
| H36 | R | Completion ratio visible | Agents can see done vs started (task checks, verify status, or an optional metric). |

## L08 — Feature list primitive (H37–H42)

| ID | Sev | Intent | Pass |
|---|---|---|---|
| H37 | R | Evidence on units | Completing a unit leaves evidence (`reports/`, links, commits) — not unchecked claims. |
| H38 | R | Verify gate exists | A Specboot verify skill/command gates “done” against change artifacts. |
| H39 | R | Verify gate runnable | That gate has a documented invocation path. |
| H40 | R | No self-marking pass | Verify/docs tasks are not marked done without required evidence files. |
| H41 | R | Session-sized units | Tasks are sized to finish in one session (or split when they aren’t). |
| H42 | R | Clear lifecycle | Change/task lifecycle is explicit (e.g. proposed → applying → verified → archived; checkbox progress). |

## L09 — Premature completion (H43–H48)

| ID | Sev | Intent | Pass |
|---|---|---|---|
| H43 | R | Definition of Done | Done means evidence passed — not “code written” or agent confidence. |
| H44 | R | Multi-layer verify | A mandatory ladder exists (e.g. unit → API/manual → E2E/docs as applicable). |
| H45 | R | No skipping layers | Standards/tasks forbid advancing while an earlier layer fails. |
| H46 | R | Runtime signals | Expectations cover runtime behavior (ready state, side effects, no leftover debug junk). |
| H47 | R | Repair on failure | Failed checks come with actionable fix guidance (reports, task notes, skill output). |
| H48 | R | Gate surfaces repair | The verify path reports failures and how to repair them. |

## L10 — E2E and architecture (H49–H56)

| ID | Sev | Intent | Pass |
|---|---|---|---|
| H49 | R | E2E runnable | When UI or cross-boundary behavior matters, a documented E2E path exists. |
| H50 | R | Arch check available | Architectural constraints can be checked (mechanical script and/or enforced OpenSpec/`docs` rules). |
| H51 | R | Arch checker exists | If mechanical checks are used, the checker is present and runnable. |
| H52 | R | Arch rules registered | Boundary rules live in `docs/`, OpenSpec config, and/or a machine-readable registry. |
| H53 | R | Violations explain fix | Arch failures state what broke, why it matters, and how to fix. |
| H54 | R | Boundaries in standards | Layer/boundary rules are written for agents (not only tribal knowledge). |
| H55 | R | E2E when crossing boundaries | Cross-component/domain changes require E2E (or documented N/A with rationale). |
| H56 | R | Promote review → rule | Repeated review findings are expected to become automated or documented rules. |

## L11 — Observability (H57–H63)

| ID | Sev | Intent | Pass |
|---|---|---|---|
| H57 | R | Pre-work contract | Scope and DoD are negotiated in artifacts before build (`proposal`/`design` or equivalent). |
| H58 | R | Evaluator bar | An explicit review bar exists (adversarial-review skill, checklist, rubric, …). |
| H59 | R | Session signals | Structured evidence of what ran in a session exists when useful (reports, traces, logs). |
| H60 | R | Trace storage | If traces are collected, they have a defined home (and usually are gitignored if runtime-only). |
| H61 | R | Observability protocol | Standards describe scope → implement → verify → review flow. |
| H62 | R | Review bar referenced | Standards point agents at the evaluator/review mechanism. |
| H63 | R | Session start/end hooks | Clock-in/out is documented; optional automation hooks count if present. |

## L12 — Clean state (H64–H70)

| ID | Sev | Intent | Pass |
|---|---|---|---|
| H64 | R | Clean exit criteria | Session exit covers build/tests/tasks/docs and no leftover debug junk. |
| H65 | R | Clean-state checker | Optional idempotent script/command that validates exit criteria. |
| H66 | R | Clean-state runnable | That checker (if any) has a documented entrypoint. |
| H67 | R | Exit checklist in standards | Clock-out / archive steps reference clean exit expectations. |
| H68 | R | Module quality scores | Optional doc scoring module health to prioritize cleanup. |
| H69 | R | Quality doc referenced | If H68 exists, standards point to it. |
| H70 | R | Dual-mode cleanup | Immediate cleanup every session plus periodic drift sweep is documented. |

---

## Critical subset

H01, H02, H03, H14, H19, H24, H25 — seven must-have intents. Judge only the Pass column.
