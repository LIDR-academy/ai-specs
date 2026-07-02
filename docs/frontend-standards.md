---
description: Frontend and UI standards for the current EdgeOps AI product surfaces in vmw-rag.
globs:
  - "deploy/prod-vm/nginx/html/**/*"
  - "src/vmware_mcp/admin/templates/**/*"
  - "src/capacity_model/dashboard/**/*"
  - "deploy/prod-vm/nginx/conf.d/*.conf"
alwaysApply: true
---

# Frontend Standards

## 1. What Counts as Frontend in This Project

The current frontend surface is multi-style and proxy-driven. It includes:

- Static portal and static operational dashboards served by Nginx
- Admin pages rendered from Jinja templates on the MCP server
- The Dash-based capacity dashboard
- Integrated third-party product UIs:
  - LibreChat
  - Docmost

This is not a React-first or SPA-first repository.

## 2. Default UI Strategy

- Prefer the lightest surface that matches the need.
- Use static HTML and JS for portal-style landing pages and simple dashboards.
- Use Jinja templates for authenticated operational/admin workflows that belong to the MCP server.
- Use Dash only for data-heavy capacity workflows that already live in that subsystem.
- Do not introduce a new frontend framework by default.

## 3. Public Surface Model

- Root domain:
  - portal landing page
  - tool catalog
  - help and changelog
  - VCO dashboards
  - maintenance-impact pages
- `chat.<apex-domain>`:
  - LibreChat
- `docs.<apex-domain>`:
  - Docmost with SSO bridge
- `capacity.<apex-domain>`:
  - Dash capacity dashboard
- `api.<apex-domain>`:
  - MCP and HTTP API surface
- `inspector.<apex-domain>`:
  - MCP Inspector for controlled MCP testing and debugging

Any UI change that affects navigation, auth, or cross-surface movement must account for the Nginx gateway behavior.

## 4. Navigation and Layout Rules

- Treat the shared portal navigation as a platform concern.
- If a page is part of the public suite, ensure it works with the current Nginx injection or hosting pattern.
- Keep operator-facing interfaces explicit and status-rich.
- Favor operational clarity over decorative design.
- For request-submission flows, show the server-generated outcome and correlation context. Do not imply success until the backend has returned the Jira key or explicit partial-success state.

## 5. Auth and Session Expectations

- Public product surfaces are protected by OAuth2 Proxy and domain routing.
- Some internal identity flows depend on forwarded headers such as `X-Email`.
- Do not build UIs that assume direct localhost-only access if the real user path is behind Nginx and subdomains.

## 6. Surface-Specific Rules

### Static Portal and Dashboards

- Keep dependencies minimal.
- Avoid unnecessary client-side frameworks.
- Build for direct Nginx hosting and subpath routing.

### Admin Templates

- Keep templates server-driven and operational.
- Prefer simple forms, clear tables, and explicit action outcomes.
- If adding a workflow, keep auth requirements and route ownership obvious.

### Capacity Dashboard

- Preserve the Dash multi-page structure already in use.
- Changes must respect current dashboard navigation and exported-report behavior.
- Browser-facing capacity features should remain inside the existing dashboard subsystem unless there is a strong reason to move them.

## 7. Verification

- Verify the real rendered surface for UI-affecting changes.
- If a page depends on proxy behavior, verify through the proxied route when possible.
- For static portal changes, verify the built page plus the Nginx-served path.

## 8. Historical Boundaries

- Older Streamlit interfaces are not the default UI target for new work.
- If historical UI code remains useful as reference, label it as legacy context rather than current architecture.
