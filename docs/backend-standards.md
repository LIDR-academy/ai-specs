---
description: Backend architecture, implementation patterns, and operational rules for the current vmw-rag EdgeOps AI stack.
globs:
  - "src/**/*.py"
  - "deploy/prod-vm/**/*"
  - "docker/**/*.yml"
  - "tests/**/*.py"
  - "pyproject.toml"
  - "Makefile"
alwaysApply: true
---

# Backend Standards

## 1. Active Backend Stack

- Language: Python 3.11+
- Core server runtime: FastMCP plus mounted HTTP routes
- Main HTTP transport: streamable HTTP on the MCP server
- Public ingress: Nginx plus OAuth2 Proxy
- Primary backend entrypoint: `src/vmware_mcp/run_mcp_http.py`
- MCP/HTTP app surfaces:
  - `src/vmware_mcp/fastmcp_server.py`
  - `src/vmware_mcp/streamable_http.py`
  - `src/vmware_mcp/admin/routes.py`
  - `src/vmware_mcp/vco/vco_dashboard_api.py`
  - `src/vmware_mcp/jira/firewall_request_api.py`
- Capacity backend: Dash app in `src/capacity_model/dashboard/`

## 2. Architecture Rules

- Extend the current mounted MCP server before adding new standalone servers.
- Prefer one operational control plane on port `8506` over parallel ad hoc APIs.
- Treat Nginx routing and OAuth2 Proxy behavior as part of the backend contract.
- If a feature is public on a subdomain, its proxy route is part of the implementation, not an afterthought.

## 3. MCP Tool Patterns

- MCP tools are part of the active platform contract.
- New tool behavior must fit the existing FastMCP registration model.
- Reuse existing domain modules and registration flow instead of creating isolated sidecar APIs without strong reason.
- Respect risk classification and approval workflows for destructive or high-impact operations.
- If a tool changes request shape, output shape, risk tier, or discoverability, update the tool-facing docs and API spec.

## 4. HTTP Endpoint Patterns

- Current public/backend HTTP surfaces include:
  - `/mcp`
  - `/health`
  - `/tools`
  - `/capabilities`
  - `/auth/*`
  - `/admin/*`
  - `/api/vco/*`
  - `/api/firewall-requests/velops`
- Browser-oriented auth and identity routes live behind Nginx and commonly depend on the `X-Email` header from OAuth2 Proxy.
- Portal form routes that create Jira tickets also depend on `X-Email`; do not trust client-submitted identity fields for submitter attribution.
- Keep JSON response shapes explicit and stable for routes used by portal pages and operational dashboards.
- Prefer mounted routes on the existing app over introducing incompatible parallel behavior.

## 5. Persistence Model

The active stack uses polyglot persistence. Keep boundaries explicit.

- Qdrant:
  - knowledge retrieval and vector search
  - production collection family centered on `vmware_knowledge`
- SQLite:
  - local operational state such as `data/ops.db`
  - capacity-model persistence with WAL mode
- PostgreSQL:
  - observability and operational relational storage
- MongoDB:
  - LibreChat application state
- Docmost Postgres and Redis:
  - documentation product state and caching
- MongoDB, Docmost Postgres, and Docmost Redis are product-owned stores. Do not write operational platform state into those stores from MCP code.

- Do not collapse unrelated stores into one datastore without a clear operational reason.
- Document every new table, collection, or persistent file in `docs/data-model.md`.

## 6. Deployment Rules

- The authoritative prod deployment flow is `scripts/deploy-prod.sh`.
- Deployment assumptions must match what that script actually does:
  - deploy the revision on `origin/main`
  - sync or archive the repo to `/opt/edgeopsai/edgeopsai`
  - refresh Compose services from `deploy/prod-vm/docker-compose.yml`
  - reload Nginx without assuming full container recreation
  - verify the deployed revision and live endpoints
- If a code change requires new environment variables, volumes, domains, or services, update the prod deploy artifacts in the same change.
- The same Compose file is used for prod and nonprod; environment-specific behavior must come from `.env` values, generated Nginx templates, or mounted runtime secrets.

## 7. Testing Standards

- Use `pytest` for Python verification.
- Run focused tests first, then broader coverage when risk justifies it.
- When backend changes affect deployed routing or health behavior, verify at least one of:
  - direct local route behavior
  - Nginx-proxied route behavior
  - deploy-time health checks
- Prefer deterministic tests over broad smoke tests.

## 8. Safety and Auth

- Preserve JWT- and OAuth2-based auth behavior across the MCP, portal, and admin surfaces.
- New admin or browser-facing routes must define their auth model explicitly.
- Destructive actions must remain auditable and approval-aware.
- Production-safe defaults matter more than local convenience.

## 9. Historical Boundaries

- Older Streamlit/OpenWebUI/backend variants remain in the repo as historical or auxiliary artifacts.
- Do not use them as the default template for new backend work unless the current deploy flow actively routes to them.
