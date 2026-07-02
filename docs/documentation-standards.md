---
description: Standards for maintaining project documentation and OpenSpec-facing technical context in vmw-rag.
alwaysApply: true
---

# Documentation Standards

## 1. Purpose

These docs are not generic examples. They are active technical context for AI agents and developers working on the current EdgeOps AI stack.

## 2. General Rules

- Write all documentation in English.
- Keep documentation aligned with the current deployed architecture.
- Prefer concrete file paths, routes, domains, and runtime behavior over abstract descriptions.
- If the repo has legacy and current implementations side by side, document which one is current.

## 3. OpenSpec Context Rules

- `docs/` is the project context layer for OpenSpec-driven work.
- New architecture, API, workflow, or data changes must update the relevant `docs/` file in the same change.
- Do not leave OpenSpec to infer current behavior from stale historical docs if the current stack is known.

## 4. Historical Workflow Handling

- OpenSpec is current.
- GSD and `.planning/` are historical.
- Historical material can be referenced for background but must not be described as the active workflow.

## 5. Required Documentation Updates by Change Type

### If you change API routes or public HTTP behavior

Update:

- `docs/api-spec.yml`
- any affected backend or frontend standards
- relevant deploy or operations docs if the public gateway changes

### If you change entities, persistence, or storage boundaries

Update:

- `docs/data-model.md`
- backend standards if the storage pattern changes

### If you change workflow, planning, or implementation conventions

Update:

- `docs/base-standards.md`
- `docs/development_guide.md`

### If you change domains, subdomains, ingress, or deployment behavior

Update:

- `docs/backend-standards.md`
- `docs/frontend-standards.md`
- `docs/development_guide.md`
- any relevant deploy docs

## 6. Live-Stack-First Rule

When older docs conflict with the current platform:

1. Check `scripts/deploy-prod.sh`
2. Check `deploy/prod-vm/docker-compose.yml`
3. Check `deploy/prod-vm/nginx/conf.d/*.conf`
4. Check the active server entrypoints and mounted routes
5. Update the docs so future work starts from the corrected context

## 7. Writing Style

- Use direct, implementation-ready language.
- Avoid marketing language.
- Avoid placeholder examples when real project examples are available.
- Make explicit whether something is current, legacy, optional, or auxiliary.
