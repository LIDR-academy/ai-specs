---
name: auditing-agent-harness
description: Use when auditing Specboot agent-harness maturity on the 70-point checklist (H01–H70), when pressure appears to add parallel state files that duplicate OpenSpec/docs/ai-specs, or when the user asks for an X/70 harness score.
author: LIDR.co
version: 1.3.0
---

# auditing-agent-harness

Score **harness intent** for a **Specboot** repo on Specboot’s fixed **70-point** rubric (`references/harness-checklist.md`).

**Specboot** (LIDR): OpenSpec + technical context under `docs/` + skills/agents/commands under `ai-specs/` (mirrors symlink in). Discover project-specific names per repo.

**Iron law:** Never create parallel state files (`PROGRESS.md`, `DECISIONS.md`, `feature_list.json`, template Makefiles) to fake checklist points. Score real Specboot artifacts; only then name **real** gaps.

**Violating the letter of the rules is violating the spirit of the rules.**

## When to use

- “Audit the agent harness” / “how do we score on the 70 points?”
- Pressure to add duplicate progress/decision/feature-list files beside OpenSpec
- Comparing ad-hoc harness advice to OpenSpec / `docs` / `ai-specs`

**Not for:** ordinary OpenSpec apply/verify, or a canned improvement roadmap from another chat.

## Workflow

1. Load `references/harness-checklist.md` (H01–H70). That file is the rubric and SoR for this skill.
2. **Discover** this repo’s Specboot surfaces (router, runbook, `openspec/`, `ai-specs/`, green commands).
3. For each point, mark `pass` / `partial` / `fail` using the **Pass** column.
4. **Score:** passes + 0.5×partials out of 70 (or passes/70 + partial list). Also report critical subset (H01, H02, H03, H14, H19, H24, H25).
5. Gaps = `fail`/`partial` intents still missing. Prefer strengthening Specboot artifacts over inventing parallel files.
6. Never name non-canonical parallel filenames in project standards — not even as “do not invent X”. Say what **to** use.

Optional short pattern overview: `references/equivalence-map.md`.

## Router vs runbook size (H08)

Long root runbook + short `docs/` router = valid. Do not merge/amputate for a line-count heuristic.

## Red flags — STOP

- Creating parallel state files to “pass” checklist rows
- Skipping the checklist and “vibes” scoring
- Treating one product’s paths or a prior chat’s tips as Specboot standard
- Pasting a fixed “top N improvements” list from this skill or another session
- Putting “do not invent …” bans for unused filenames into `docs/`

## Rationalizations

| Excuse | Reality |
|---|---|
| “CRITICAL ⇒ create that file” | Map to Pass criteria; use OpenSpec/`docs`/`ai-specs` first |
| “Warn agents about PROGRESS.md” | Naming unused files teaches the wrong SoR |
| “Reuse last project’s tips” | Re-score H01–H70 on *this* repo |
| “Close enough without the checklist” | Output must be an X/70 against H01–H70 |

## Output shape

1. Score: **X / 70** (pass + 0.5 partial) + critical subset
2. Fails/partials with discovered paths and IDs (Hxx)
3. Real capability gaps only — if none, say so; no canned roadmap
