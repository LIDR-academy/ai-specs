# Data Model

This project uses polyglot persistence. The current data model is split across
operational metadata, vector knowledge search, product state, and capacity
planning storage.

## 1. Core Domain Concepts

### Platform Surface

Represents the main product entrypoints exposed behind Nginx:

- `portal`: root domain landing page and static dashboards
- `api`: MCP and HTTP API gateway
- `chat`: LibreChat
- `docs`: Docmost
- `capacity`: Dash capacity dashboard
- `inspector`: MCP Inspector for controlled MCP testing

These are routing and access-control concepts rather than database tables, but
they are central to the deployed platform model.

### POP

Represents a point of presence and its infrastructure identity.

Typical attributes:

- `code`
- `region`
- `environment`
- `vcenter_host`
- `aliases`
- `active`

Primary source:

- `src/vmware_mcp/pops.yaml`

### Auth User

Represents a platform user defined in config-backed auth.

Typical attributes:

- `username`
- `email`
- `role`
- `password_hash` or token-derived identity
- `jira` or auxiliary integration settings

Related concept:

- `subdomain_access`: maps roles to allowed surfaces such as `chat`, `docs`,
  `capacity`, `api`, `inspector`, and `portal`

Primary source:

- `src/config.yaml`

## 2. Operational Control and Audit Model

### Approval Request

Represents approval state for high-risk operations.

Typical attributes:

- `id`
- `operation`
- `resource_type`
- `resource_id`
- `vcenter_host`
- `risk_level`
- `status`
- `requester`
- `approver`
- `created_at`
- `expires_at`
- `approved_at`
- `executed_at`
- `operation_params`

Current implementation:

- in-memory approval store plus audit logging in the MCP server

### Audit Entry

Represents an auditable event associated with approval and execution flow.

Typical attributes:

- `operation`
- `actor`
- `resource_type`
- `resource_id`
- `vcenter_host`
- `status`
- `risk_level`
- `approval_id`
- `details`

## 3. SQLite Operational State

### `job_runs`

Stored in `data/ops.db`.

Tracks scheduled or proactive job execution.

Fields:

- `id`
- `tool`
- `vcenter`
- `started_at`
- `finished_at`
- `status`
- `output_summary`

### `corpus_status`

Stored in `data/ops.db`.

Tracks freshness and point counts for indexed knowledge sources.

Fields:

- `source_type`
- `last_ingest`
- `point_count`
- `status`
- `updated_at`

### LangGraph Checkpoints

Also stored alongside `ops.db` through the SQLite checkpointer.

Purpose:

- investigation and agent workflow checkpoint persistence

## 4. PostgreSQL Observability Model

### `llm_requests`

Stored in the `observability` PostgreSQL database.

Tracks LLM usage and failures from the LiteLLM sidecar.

Fields include:

- `id`
- `ts`
- `model`
- `api_base`
- `status`
- `input_tokens`
- `output_tokens`
- `total_tokens`
- `latency_ms`
- `cost_usd`
- `user_email`
- `error_message`
- `error_type`
- `request_id`
- `litellm_call_id`

### `mcp_tool_calls`

Stored in the `observability` PostgreSQL database.

Tracks MCP invocation metadata.

Fields include:

- `id`
- `ts`
- `tool_name`
- `status`
- `latency_ms`
- `pop_code`
- `vcenter_host`
- `user_email`
- `request_id`
- `error_message`
- `error_category`

## 5. Qdrant Knowledge Model

### Knowledge Point

Represents a searchable vectorized knowledge fragment in Qdrant.

Typical attributes:

- vector embedding
- document text or chunk text
- `source_type`
- source metadata such as product, article, URL, or document origin

Current role:

- hybrid knowledge retrieval for VMware docs, KBs, Arista sources, and related
  operational knowledge

## 6. Capacity Model Storage

The capacity dashboard uses a dedicated SQLite store with WAL mode.

### `pop_snapshots`

Represents daily POP capacity state.

Fields include:

- `pop_id`
- `snapshot_date`
- `vcg_count`
- `host_count`
- `vvf_hosts`
- `vvf_base_hosts`
- `cpu_utilization_pct`
- `memory_utilization_pct`
- `vcg_headroom`
- `max_vcg_capacity`
- `server_model`
- `capacity_status`
- `limiting_factor`
- `months_to_exhaustion`
- `vm_inventory`

### `collection_runs`

Tracks capacity collection executions.

Fields include:

- `run_id`
- `started_at`
- `completed_at`
- `pops_attempted`
- `pops_succeeded`
- `failed_pops`
- `duration_seconds`
- `status`

### `unknown_vm_first_seen` and `unknown_vm_diffs`

Track unknown VM staleness and per-snapshot add/remove history.

Key fields:

- `pop_id`
- `vm_name`
- `first_seen_date`
- `snapshot_date`
- `change_type`

### `netbox_reconciliation`

Stores vCenter-to-NetBox reconciliation results for dashboard views.

Key fields:

- `pop_id`
- `snapshot_date`
- `vm_name`
- `source`
- `match_status`
- `match_strategy`
- `netbox_role`
- `dashboard_category`

### `vm_exclusions`

Stores user-managed capacity dashboard exclusions.

Fields:

- `pop_id` (`NULL` means global)
- `vm_name`
- `excluded_at`
- `reason`

### `cloud_vcg_snapshots`

Stores cloud and colo VCG inventory classified by platform.

Fields include:

- `snapshot_date`
- `name`
- `platform`
- `cluster_name`
- `cluster_type`
- `site_slug`
- `status`
- `custom_fields`

### `vco_vcg_associations`

Stores VCO-to-site association data with VCG counts.

Fields include:

- `snapshot_date`
- `site_slug`
- `vco_name`
- `vco_fqdn`
- `vco_version`
- `vco_status`
- `vco_tags`
- `vco_host_type`
- `vcg_count_at_site`

### `vcg_service_snapshot`

Stores VCO-derived service state for VCGs by site.

Fields include:

- `snapshot_date`
- `site_slug`
- `total_count`
- `in_service_count`
- `assigned_count`
- `vapp_disabled_count`

### `site_locations`

Stores geographic metadata used by map and geo-coverage views.

Fields:

- `site_slug`
- `name`
- `latitude`
- `longitude`
- `region`
- `environment`
- `source`
- `updated_at`

## 7. External Product State

### LibreChat MongoDB

LibreChat owns its MongoDB schema. Treat it as product state for users,
conversations, sessions, and chat configuration. MCP code should integrate
through supported service APIs and configuration, not by writing LibreChat
MongoDB documents directly.

### Docmost Postgres and Redis

Docmost owns its Postgres schema and Redis cache. The platform integrates with
Docmost through deployment configuration, SSO bridge behavior, and Drive sync
jobs; do not treat these stores as general operational databases.

### Jira VELOPS Issues

Portal firewall requests create Jira issues in the `VELOPS` project with issue
type `GCP Firewall Rule Change`. The submitted form data is rendered into the
issue summary, description, labels, and optional attachments. The submitter is
derived from the authenticated `X-Email` header forwarded by Nginx/OAuth2 Proxy.
