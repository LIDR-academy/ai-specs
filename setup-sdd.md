# Spec-Driven Development Setup Instructions for LIDR Specboot

**Purpose**: These instructions tell any coding agent how to install and configure this repository's OpenSpec-aligned workflow into any target project automatically.

Repository source:
- `https://github.com/LIDR-academy/lidr-specboot`

Raw setup file URL:
- `https://raw.githubusercontent.com/LIDR-academy/lidr-specboot/main/setup-sdd.md`

---

## What the Coding Agent Must Do

Execute these steps in order. Perform the setup directly; do not ask the user to run commands unless a manual approval is required. **Exception:** step 12 (RTK) requires prompting the user for their AI tool before running `rtk init`; if they do not answer, use `rtk init -g` (Claude Code / Copilot default).

### 1) Preflight Validation

Verify all of the following in the current working directory (target project root):

1. It is a git repository (`git rev-parse --is-inside-work-tree`)
2. User has write permissions
3. `node` is available and version is `>= 20.19.0`
4. `npm` is available

If any check fails, stop and report exactly what is missing.

### 2) Detect Existing Specboot Installation Channel

This setup must work whether `specboot` was installed from npm or provided by a Claude Code plugin.

Detection and compatibility rules:

1. If the project already contains any of these paths, treat Specboot as already applied and skip re-import:
   - `ai-specs/`
   - `docs/base-standards.md`
   - `CLAUDE.md` or `AGENTS.md` symlinked to `docs/base-standards.md`
2. If the user says Specboot was installed via plugin, do **not** require a global npm install of `@lidr/lidr-specboot`.
3. If no Specboot artifacts exist yet, use the automated bootstrap command in step 5.

### 3) Install OpenSpec (if needed)

If `openspec` is not available, install it globally:

```bash
npm install -g @fission-ai/openspec@latest
```

Re-verify with:

```bash
openspec --version
```

### 4) Initialize OpenSpec (if needed)

If the project does not have OpenSpec initialized yet, run:

```bash
openspec init
```

If it is already initialized, keep existing artifacts and continue.

### 5) Import LIDR Specboot Into the Project

Copy this repository's baseline files into the target project **without overwriting existing files**.
Keep the source `README.md` as onboarding instructions inside `ai-specs/` instead of replacing the project's root README.

Recommended automation sequence:

```bash
tmp_dir="$(mktemp -d)"
git clone --depth 1 https://github.com/LIDR-academy/lidr-specboot.git "$tmp_dir/lidr-specboot"
cp -rn "$tmp_dir/lidr-specboot/"* .
mkdir -p ai-specs
cp -n "$tmp_dir/lidr-specboot/README.md" "ai-specs/specboot-instructions.md"
rm -rf "$tmp_dir"
```

Important:
- Use `cp -rn` exactly to preserve existing project files (especially existing root `README.md`)
- Store template onboarding instructions at `ai-specs/specboot-instructions.md` for future setup reference
- Keep hidden directories such as `.claude/` and `.cursor/` when present in source
- Remove temporary clone after copy (`rm -rf "$tmp_dir"`)

### 6) Ensure OpenSpec Config Includes This Technical Context

Update the project's OpenSpec config file (`config.yml` or `openspec/config.yaml`, whichever exists) so context includes:

- `docs/base-standards.md` as single source of truth
- `docs/backend-standards.md`
- `docs/frontend-standards.md`
- `docs/documentation-standards.md`
- `docs/development_guide.md`
- `docs/api-spec.yml`
- `docs/data-model.md`
- `ai-specs/agents/backend-developer.md` for backend implementation guidance
- `ai-specs/agents/frontend-developer.md` for frontend implementation guidance
- `ai-specs/skills/` as reusable workflow guidance

Do not delete unrelated existing project context; merge safely.

If this project was set up through a plugin and no config file exists yet:
1. Run `openspec init`
2. Re-open `openspec/config.yaml`
3. Merge the required context entries listed above

### 7) Create a First Draft of Technical Context (When Applicable)

If the target project has missing or generic technical documentation in `docs/`, create a first draft before finishing setup.

Scope for first draft (adapt to repository reality):
- `docs/base-standards.md`
- `docs/backend-standards.md` (if backend exists)
- `docs/frontend-standards.md` (if frontend exists)
- `docs/documentation-standards.md`
- `docs/development_guide.md`
- `docs/api-spec.yml` (if API exists)
- `docs/data-model.md` (if persistence/domain model exists)

Research requirements:
1. Perform deep repository research before drafting:
   - Detect stack, frameworks, architecture, testing tools, linting/formatting, and deployment conventions
   - Inspect source folders, package manifests, lockfiles, CI workflows, tests, and existing docs
   - Cross-check findings across multiple files before writing conclusions
2. Preserve existing project decisions; do not invent architecture that is not present.
3. Keep all content in English.

Prompt example (use with your coding agent to guarantee deep research):

```text
Create a first draft of this project's technical context in docs/ using the same structure and file set from LIDR Specboot.

Hard requirements:
- Perform deep repository research before writing anything.
- Analyze at least: package manifests, lockfiles, source tree, tests, CI/workflows, lint/format configs, and current documentation.
- Infer real stack, architecture, coding conventions, test strategy, API shape, and domain model from evidence in the repository.
- Keep existing structure and file names in docs/; do not change the template structure.
- If a section is missing evidence or is ambiguous, ask concise clarification questions first (short-answer format), then continue.
- Do not use placeholders like "TBD" when evidence exists in the codebase.
- Keep all technical artifacts in English.

Deliverables:
1) Updated docs/base-standards.md
2) Updated docs/backend-standards.md and/or docs/frontend-standards.md where applicable
3) Updated docs/documentation-standards.md
4) Updated docs/development_guide.md with project-specific setup, environment, and workflows
5) Updated docs/api-spec.yml and docs/data-model.md where applicable
6) A short evidence summary listing which files were used to infer each major decision
```

Missing section policy (mandatory):
- When a required section is missing or unclear, ask short clarification questions and wait for answers before finalizing that section.
- Use short-answer questions (one line each), for example:
  - "Backend framework? (Express/Fastify/Nest/Other)"
  - "Primary database? (PostgreSQL/MySQL/MongoDB/Other)"
  - "Frontend framework? (React/Vue/Angular/Other)"
  - "Testing stack? (Jest/Vitest/Cypress/Playwright/Other)"

### 8) Adapt Agent Definitions to the Project Stack

  After drafting the technical context in `docs/`, update the agent definition files
  to reflect the actual project stack. The imported agents ship with LTI template
  content (TypeScript/Express/Prisma + React) and must be rewritten to match the
  real stack discovered in step 7.

  #### Target files (canonical source — update these only)
  - `ai-specs/agents/backend-developer.md`
  - `ai-specs/agents/frontend-developer.md`

  Do not edit `.claude/agents/` or `.cursor/agents/` directly if they are symlinks
  to `ai-specs/agents/`; the canonical source update propagates automatically.
  If they are plain copies (no symlinks), update all copies.

  #### What to update in each file

  1. **Frontmatter `description` field**
     - Replace all references to the old stack (TypeScript, Express, Prisma,
       PostgreSQL, React, Bootstrap, etc.)
     - Describe the agent's expertise in terms of the actual stack detected
     - Update `<example>` blocks inside the description to use realistic scenarios
       for this project (entity names, file paths, patterns from the real codebase)

  2. **Body — persona and expertise**
     - Rewrite the "Your Core Expertise" and "Architectural Principles" sections
       to match the real stack, frameworks, and conventions documented in
       `docs/backend-standards.md` and `docs/frontend-standards.md`
     - Preserve the agent's behavioral contract: propose-first, save plan to file,
       never implement directly

  3. **`product-strategy-analyst.md`** — stack-agnostic; no changes needed.

  #### Research requirement
  Use the same stack evidence gathered in step 7 (package manifests, entities,
  controllers, components, config files). Do not invent patterns not present in
  the repository.

  #### Completion condition
  Both agent files must no longer reference the template stack. Verify by checking
  that `backend-developer.md` does not mention Prisma/Express/TypeScript-backend,
  and `frontend-developer.md` does not mention React/Bootstrap/axios.


### 9) Find and Fix Agentic Setup Gaps

After Specboot scaffolding is in place, perform a **deep gap analysis** to detect
anything in the target project's existing agentic setup that is misaligned with
the new canonical layout (`docs/` + `ai-specs/`). Do not stop at listing gaps —
**implement the fix for every gap found** (or every gap that can be resolved
without user input; see contradiction policy below).

#### 9a) Analysis scope

Inspect at minimum:

1. **Legacy instruction locations**
   - `.github/instructions/`
   - `.github/agents/`
   - Root or nested `AGENTS.md`, `CLAUDE.md`, `codex.md`, `GEMINI.md` (when not
     already symlinks to `docs/base-standards.md`)
   - Agent-specific folders: `.claude/`, `.cursor/`, and any other configured
     agent paths (for example `.gemini/`, `.codex/`)
2. **Skills outside canonical source**
   - Any `skills/` directory outside `ai-specs/skills/`
   - Repo-specific skills (for example `flat-i18n`) living under agent folders
3. **Duplicated or divergent standards**
   - Content repeated across `docs/`, `AGENTS.md`, `.github/instructions/`, and
     agent config files
   - Contradictions between legacy instructions and Specboot standards
4. **Symlink policy violations**
   - Real files or directories where Specboot expects symlinks into `ai-specs/`
   - Broken or stale symlinks after migration
5. **Supplementary documentation folders**
   - Project-specific doc trees outside canonical `docs/` (for example
     `internal-docs/`, `architecture/`, `deployment/`, `uml/`, `diagrams/`)
   - ADR collections not yet under `docs/adr/`
   - UML, flow, or sequence diagrams stored outside `docs/`
   - Local environment setup, cloud deployment, or operations runbooks in
     non-canonical locations
   - `README.md` sections (root or nested) that duplicate agent rules or
     standards already covered in `docs/`

Research requirements:

- Cross-check findings across all locations above before concluding a gap exists.
- Prefer evidence from file contents and git history over assumptions.
- Record each gap as: **Gap**, **Impact**, **What to do**, **Status**.

#### 9b) Known gap patterns (generalized from real implementations)

Use these as high-priority checks; also look for project-specific variants.

| Gap | Impact | What to do |
| --- | --- | --- |
| Legacy knowledge still in `.github/instructions/` and `.github/agents/` | Duplicated or disconnected from canonical `docs/` + `ai-specs/`; agents may follow stale rules | Move current instructions into `docs/base-standards.md` or the appropriate sub-file (`backend-standards.md`, `frontend-standards.md`, `documentation-standards.md`). Keep `base-standards.md` as short as possible — link to sub-files instead of inlining everything. Remove duplicates. When contradictions are found, **ask the user** which rule should win before deleting content. After migration, remove or replace legacy files so they are not a second source of truth. |
| Repo-specific skills live outside `ai-specs/skills/` | Breaks canonical-source policy; skills drift across agents | Move each skill to `ai-specs/skills/<skill-name>/`. Replace the original location with a symlink to the canonical skill. Create matching symlinks in every configured agent mirror (for example `.claude/skills/` and `.cursor/skills/`). Run the `sync-agent-symlinks` skill to verify mirrors. |
| Valuable content in root `AGENTS.md` (or other agent entry files) before symlink replacement | Replacing the file with a symlink to `base-standards.md` would drop project-specific guidance | Extract instructions first: merge into `docs/base-standards.md` or the right sub-file using the same short-base + linked-sub-files approach. Remove duplicates. **Ask the user** when contradictions appear. Only then replace the root file with a symlink to `docs/base-standards.md` (or keep a minimal pointer file if the agent runtime requires it). |
| Technical context in supplementary doc folders (for example `internal-docs/`) | Second doc tree outside canonical `docs/`; agents may miss ADRs, deployment guides, environment setup, or workflow runbooks | Classify each file: standards → `docs/*-standards.md`; runbooks and environment setup → `docs/*-guide.md` or `docs/development_guide.md` with links; ADRs → `docs/adr/`; diagrams → `docs/diagrams/` (or linked from the relevant guide). Register all canonical paths in `docs/base-standards.md` (links only) and `openspec/config.yaml`. Migrate content before removing the legacy folder. |
| Long guide and short summary coexist for the same topic | Duplication or drift between summary and deep guide | Keep the short summary in `docs/development_guide.md` (or the relevant standards file). Move the deep guide to `docs/<topic>-guide.md`. Link from the summary; never inline long guides into `base-standards.md`. |
| Authoring or tooling docs differ from runtime behavior docs (for example CLI workflow vs API-fetched data) | Apparent contradictions that are actually layered concerns, or real conflicts if both claim the same runtime contract | Document both layers explicitly in the appropriate standards file (`frontend-standards.md`, `backend-standards.md`, etc.) with clear boundaries (tooling vs runtime). **Ask the user** only when both sources claim to be the single source of truth for the same behavior. |

Add any additional gaps discovered during analysis to the same table format before
fixing them.

#### 9c) Contradiction and safety policy

- **Contradictions**: stop automated deletion; present a short summary to the user
  and wait for a decision when two sources disagree on stack, conventions, or
  workflow rules.
- **Non-symlink conflicts**: do not overwrite real directories in agent mirrors;
  report as `conflict` (same rules as `sync-agent-symlinks`).
- **Preserve value**: migrate content before removing legacy paths; never delete
  instructions that have not been relocated or explicitly rejected by the user.
- **Layered documentation**: when supplementary doc folders and canonical `docs/`
  describe the same concern at different layers (for example tooling workflow vs
  runtime behavior, or a deep runbook vs a short summary), prefer documenting
  both layers with clear boundaries rather than deleting one source. Ask the user
  only when both claim to be the single source of truth for the same behavior.

#### 9d) Implementation (mandatory)

After the gap inventory is complete:

1. Apply every **What to do** action for gaps that are safe to resolve.
2. For skills moved into `ai-specs/skills/`, run `sync-agent-symlinks` and fix
   any broken or missing mirror symlinks.
3. Re-run symlink integrity checks from step 10 for affected paths.
4. Produce a gap report listing: gaps found, actions taken, items deferred to
   the user (contradictions), and files removed or replaced.

Do not mark setup complete while unresolved canonical-source violations remain,
except when blocked by an explicit user decision on a contradiction.


### 10) Verify Symlink Integrity and Key Files

Verify that the imported structure is usable:

1. Root agent config files exist:
   - `AGENTS.md`
   - `CLAUDE.md`
   - `codex.md`
   - `GEMINI.md`
2. Core standards exist:
   - `docs/base-standards.md`
3. Skills and agents exist:
   - `ai-specs/skills/`
   - `ai-specs/agents/`
4. If `.claude` / `.cursor` symlinks exist, ensure they are not broken.

### 11) Install CodeGraph (Recommended)

CodeGraph provides a pre-indexed local code knowledge graph (MCP tools) that reduces exploration tool calls and token usage. Repository: https://github.com/colbymchenry/codegraph

#### 11a) Install the CLI (if needed)

If `codegraph` is not available, install it:

```bash
# macOS / Linux (recommended — no Node.js required)
curl -fsSL https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.sh | sh

# Windows (PowerShell)
irm https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.ps1 | iex

# Alternative when Node.js is already available
npm install -g @colbymchenry/codegraph
```

Re-verify with:

```bash
codegraph --version
```

If the command is not found immediately after install, open a new shell or ensure `~/.local/bin` is on `PATH`.

#### 11b) Wire up the agent (auto-detect)

CodeGraph **auto-detects** installed agents. Run the installer from a shell where `codegraph` resolves:

```bash
codegraph install
```

The installer detects and configures Claude Code, Cursor, Codex CLI, opencode, Hermes Agent, Gemini CLI, Antigravity IDE, and Kiro — wiring the CodeGraph MCP server into each.

For non-interactive setup (CI or scripted installs), use:

```bash
codegraph install --yes                              # auto-detect agents, install global
codegraph install --target=cursor,claude --yes       # explicit target list
```

After install, remind the user to **restart their AI tool** so the MCP server loads.

#### 11c) Initialize the project index

In the target project root, build the local knowledge graph:

```bash
codegraph init
```

This creates `.codegraph/` and indexes the repository. Auto-sync is enabled by default on file changes.

Verify with:

```bash
codegraph status
```

If initialization fails, report the exact error and continue with the remaining setup steps when possible.

### 12) Install RTK (Recommended)

RTK is a CLI proxy that compresses common dev-command output (git, tests, grep, etc.) to reduce LLM token consumption by 60–90%. Repository: https://github.com/rtk-ai/rtk

#### 12a) Install the CLI (if needed)

If `rtk` is not available, install it:

```bash
# Homebrew (macOS/Linux — recommended when available)
brew install rtk

# Quick install (macOS/Linux)
curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/master/install.sh | sh
```

Re-verify with:

```bash
rtk --version
```

If `rtk gain` fails, the wrong `rtk` package may be installed (name collision with Rust Type Kit on crates.io). Reinstall from the GitHub source above.

#### 12b) Initialize hooks for the user's agent

Unlike CodeGraph, RTK requires an **agent-specific** init command. **Prompt the user** which AI tool they use before running init.

Use a short multiple-choice prompt, for example:

```text
Which AI coding tool should RTK configure?
- Claude Code / GitHub Copilot (default)
- Cursor
- Codex (OpenAI)
- Gemini CLI
- Windsurf
- Cline / Roo Code
- Other (specify)
```

If the user does not specify, use the default:

```bash
rtk init -g                     # Claude Code / Copilot (default)
```

Agent-specific commands when the user chooses otherwise:

```bash
rtk init -g --agent cursor      # Cursor
rtk init -g --codex             # Codex (OpenAI)
rtk init -g --gemini            # Gemini CLI
rtk init -g --agent windsurf    # Windsurf
rtk init --agent cline          # Cline / Roo Code
rtk init --agent kilocode       # Kilo Code
rtk init --agent antigravity    # Google Antigravity
rtk init -g --agent pi          # Pi
rtk init --agent hermes         # Hermes
rtk init -g --copilot           # GitHub Copilot (VS Code)
```

After init, remind the user to **restart their AI tool** so command-rewrite hooks take effect.

Verify installation:

```bash
rtk init --show
```

### 13) Completion Output (Required)

When done, report:

1. OpenSpec status (installed + initialized)
2. Gap analysis summary (gaps found, fixes applied, contradictions deferred to user)
3. CodeGraph status (CLI installed, agent wired, project indexed)
4. RTK status (CLI installed, agent init command used)
5. Files imported (high-level summary)
6. Which config file was updated and what sections were added/merged
7. Verification results
8. Any warnings (for example, files skipped because they already existed)

---

## Post-Installation Quick Usage

After installation, suggest this workflow:

```bash
/enrich-us <ticket-or-idea>   # optional
/ff <ticket-id>
/apply <ticket-id>
/verify <ticket-id>
/archive <ticket-id>
/commit
```

Also remind the user to customize `docs/` for their real stack/domain before generating production changes.

If CodeGraph and RTK were installed, remind the user to restart their AI tool so MCP servers and RTK hooks load.

---

## Troubleshooting Rules

If setup fails:

1. Show the exact failing command
2. Explain root cause in one sentence
3. Propose the smallest safe fix
4. Continue automatically after applying the fix when possible

Do not silently skip failed steps.
