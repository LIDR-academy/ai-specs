---
description: Core engineering rules for vmw-rag and EdgeOps AI. This is the single source of truth for workflow, language, verification, and project context used by AI agents.
alwaysApply: true
---

# Base Standards

## 1. Project Context

- This repository powers the current EdgeOps AI platform across the current GCP environments.
- The current apex domains are:
  - prod: `edgeopsai.velocloud-prod.arista.com`
  - nonprod: `edgeopsai.velocloud-nonprod.arista.com`
- The active product surface is not the original standalone VMware RAG app. The current stack centers on:
  - FastMCP and mounted HTTP routes on the MCP server
  - Nginx plus OAuth2 Proxy as the public gateway
  - LibreChat on `chat.<apex-domain>`
  - Docmost on `docs.<apex-domain>`
  - The capacity dashboard on `capacity.<apex-domain>`
  - MCP Inspector on `inspector.<apex-domain>` for controlled MCP testing
  - Static portal and operational dashboards on the root domain
- Treat older Streamlit-era and GSD-era artifacts as historical context unless the code or deploy path still actively uses them.

## 2. Workflow Standard

- OpenSpec is the primary planning and execution workflow for new work.
- `.planning/`, GSD phases, `PLAN.md`, `STATE.md`, and phase numbering are historical records. Do not create new GSD workflow artifacts for new features unless a task explicitly requires backfilling history.
- For new work that changes behavior, follow this order:
  1. Update the active OpenSpec change artifacts.
  2. Update project context docs when the change affects architecture, API, workflow, or data model.
  3. Implement code.
  4. Run verification.
  5. Update the OpenSpec task state with real results.

## 3. Core Principles

- Prefer small, reviewable changes over broad rewrites.
- Ground decisions in the current deployed stack, not old repo narratives.
- Verify visible operator behavior, not only internal helper outputs.
- Keep implementation, docs, and deployment flow aligned.
- Favor explicit operational safety over cleverness.
- Use clear names and direct control flow.

## 4. Source of Truth Hierarchy

When documents disagree, resolve conflicts in this order:

1. Current production deploy path:
   - `scripts/deploy-prod.sh`
   - `deploy/prod-vm/docker-compose.yml`
   - `deploy/prod-vm/nginx/conf.d/*.conf`
2. Current runtime entrypoints and mounted routes:
   - `src/vmware_mcp/run_mcp_http.py`
   - `src/vmware_mcp/streamable_http.py`
   - `src/vmware_mcp/admin/routes.py`
   - `src/vmware_mcp/vco/vco_dashboard_api.py`
   - `src/vmware_mcp/jira/firewall_request_api.py`
   - `src/capacity_model/dashboard/app.py`
3. These project context docs in `docs/`
4. Historical docs under `.planning/` and older architecture notes

## 5. Language Standards

- All technical artifacts must be written in English.
- Use concise, implementation-ready wording.
- Avoid placeholder architecture descriptions once project-specific details are known.

## 6. Verification Standards

- Every meaningful code change must include targeted verification.
- Prefer the narrowest test that proves the change.
- If a change affects a user-facing route, dashboard, or deploy behavior, verify the visible path as well.
- If a change affects the current production stack description, update:
  - `docs/api-spec.yml`
  - `docs/data-model.md`
  - the relevant standards doc in `docs/`

## 7. Deployment Awareness

- The production deployment path is `scripts/deploy-prod.sh`.
- The nonprod deployment path is `scripts/deploy-nonprod.sh`.
- Production deploys refresh Compose services and reload Nginx on the remote VM.
- The public platform is domain-routed through Nginx, so product behavior often depends on both application code and proxy config.
- Do not describe the platform as a single prod-only domain when documenting current architecture; include nonprod domain context where relevant.
- Do not describe a service as part of the active stack unless it is deployed by the current flow for that environment or served by the current public gateway.

## 8. Related Standards

- [Backend Standards](./backend-standards.md)
- [Frontend Standards](./frontend-standards.md)
- [Documentation Standards](./documentation-standards.md)
- [Development Guide](./development_guide.md)
- [API Specification](./api-spec.yml)
- [Data Model](./data-model.md)
