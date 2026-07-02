# Development Guide

This guide describes how to work on the current EdgeOps AI stack in `vmw-rag`.
It reflects the active architecture and deployment model, not the original
standalone VMware RAG workflow.

## 1. Current Development Targets

The primary development targets are:

- FastMCP server and mounted HTTP routes
- Admin/auth routes on the MCP server
- VCO dashboard APIs
- Capacity dashboard
- Nginx routing and public platform integration
- Environment deployment flows through `scripts/deploy-prod.sh` and `scripts/deploy-nonprod.sh`

Historical GSD planning material is kept for reference only. New work should
use OpenSpec artifacts under `openspec/`.

## 2. Local Environment

### Prerequisites

- Python 3.11+
- Docker and Docker Compose
- `gcloud` access if you need to verify or deploy to the prod VM
- VPN access when testing live infrastructure calls that require it

### Activation

Use the repo activation path that exists today:

```bash
source ./activate.sh
```

### Configuration Sources

Check configuration in this order:

1. local files in the repo
2. `../shared.env` for shared credentials and environment values
3. deploy-specific files under `deploy/prod-vm/`

Do not assume every required variable lives in `.env`.

## 3. Local Runtime Paths

### Run the MCP stack through Docker

```bash
make start
```

This uses:

- `docker/docker-compose.librechat.yml`

### Run the MCP server on the host

Use this when host VPN routing matters:

```bash
make run-host
```

This is the preferred path for live vCenter testing when container networking is
not enough.

## 4. Key Files to Understand First

- `scripts/deploy-prod.sh`
- `scripts/deploy-nonprod.sh`
- `deploy/prod-vm/docker-compose.yml`
- `deploy/prod-vm/nginx/conf.d/*.conf`
- `src/vmware_mcp/run_mcp_http.py`
- `src/vmware_mcp/streamable_http.py`
- `src/vmware_mcp/admin/routes.py`
- `src/vmware_mcp/vco/vco_dashboard_api.py`
- `src/vmware_mcp/jira/firewall_request_api.py`
- `src/capacity_model/dashboard/app.py`
- `pyproject.toml`

## 5. Testing Workflow

### Targeted Python tests

```bash
python -m pytest tests/unit/<area> -q
```

### Broader verification

```bash
python -m pytest --tb=short
```

### Service checks

```bash
make status
make health
```

For UI or route changes, also verify the actual served path or proxied endpoint,
not just unit tests.

## 6. OpenSpec Workflow

Use OpenSpec for new work.

Typical sequence:

1. update the relevant OpenSpec change
2. make sure `docs/` reflects current architecture for the change
3. implement code
4. run verification
5. update tasks and reports with real results

Do not create new GSD phase artifacts for fresh work unless you are explicitly
maintaining historical records.

## 7. Production Deployment

The active deployment mechanism is:

```bash
./scripts/deploy-prod.sh
```

Important behavior in that script:

- deploys the revision currently on `origin/main`
- connects to the GCP VM `edgeopsai-prod`
- syncs the repo to `/opt/edgeopsai/edgeopsai`
- uses `deploy/prod-vm/docker-compose.yml`
- refreshes Compose services
- reloads the existing Nginx container
- verifies the deployed revision and key endpoints

If your change affects runtime behavior, make sure the deploy script and deploy
artifacts still support it.

## 8. Environment Surface Summary

Current public surfaces are driven by the environment apex domain:

- `<apex-domain>` for the root portal and static dashboards
- `chat.<apex-domain>`
- `docs.<apex-domain>`
- `capacity.<apex-domain>`
- `api.<apex-domain>`
- `inspector.<apex-domain>`

Current environment apex domains:

- prod: `edgeopsai.velocloud-prod.arista.com`
- nonprod: `edgeopsai.velocloud-nonprod.arista.com`

Use the environment-appropriate apex when describing root, `api`, `chat`,
`docs`, and `capacity` surfaces.

## 9. Documentation Expectations

When you change current behavior, update the relevant project-context docs in
the same change:

- `docs/base-standards.md`
- `docs/backend-standards.md`
- `docs/frontend-standards.md`
- `docs/documentation-standards.md`
- `docs/data-model.md`
- `docs/api-spec.yml`
