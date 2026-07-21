# Harness intent ↔ Specboot patterns (summary)

Full scoring rubric: **`harness-checklist.md` (H01–H70)**. This file is a short pattern overview only.

Use as a guide while discovering *this* repo. Do **not** treat any single product repo as the Specboot standard.

| Intent | Specboot pattern (discover locally) | How to judge |
|---|---|---|
| Agent router | `AGENTS.md` (or equivalent) → short standards under `docs/` | Pass if router + links to topic docs |
| Ops / commands | Separate runbook and/or documented project scripts | Pass if agents can find setup/dev/test |
| Topic docs | Technical context under `docs/`; OpenSpec `context` if configured | Pass if domain rules are findable |
| Skills / tools | `ai-specs/skills` (+ agents/commands); mirrors via symlink if used | Pass if skills are discoverable |
| Session state | Active OpenSpec change: `openspec/changes/<name>/tasks.md` | Pass in-flight; note idle handoff if no active change |
| Decisions | Per-change `design.md` (and lasting decisions in `docs/` if any) | Pass if rationale is tracked with the change |
| Feature list | `tasks.md` checkboxes + specs/scenarios | Pass if work units are explicit and ordered |
| Evidence / DoD | Change `reports/` + Specboot verify skill + any review skill | Pass if “done” requires evidence, not vibes |
| Layered verify | Project’s mandatory verify ladder | Pass if layers exist and ordering is enforced |
| Setup/dev/test | This repo’s real entrypoints (any runner) | Pass if one-command setup/dev/test exist *in some form* |
| Green predicate | Named lint/test + verify-when-change-active | Partial until named in standards |
| Arch / boundaries | Documented boundaries in `docs/` / OpenSpec rules; optional mechanical gate | Partial if only agent-enforced and repeatedly violated |

## Idle sessions

If `openspec/changes/` has no active change, session continuity is a soft gap only when the repo also lacks any documented clock-in (e.g. `openspec list` + green check). Do not invent parallel state files to fill it.
