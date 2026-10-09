# State

## Current task

1. Scaffold

## Tasks (one per session, in order)

1. Scaffold: uv project, src layout, ruff, mypy strict, pytest, Makefile, Dockerfile
   skeleton, .gitignore (reference/, .env, *.db), .env.example, Apache-2.0 LICENSE,
   THIRD_PARTY_NOTICES.md, CI workflow (check, gitleaks, Trivy). Clone upstream to reference/.
2. Spike, docs only: write docs/UPSTREAM-MAP.md (modules to port, endpoints, draft risk
   tier per operation). Check whether FastMCP's OAuth proxy / Azure provider covers the
   Entra facade. Confirm Key Vault Secrets User can list secret tags. Pin FastMCP.
3. Server skeleton: config.py fail-fast, FastMCP streamable-http stateless at /mcp,
   /healthz, JSON logger that never logs bodies or headers. Tests.
4. Redaction: sanitize(), denylist, entropy net, synthetic golden fixtures with planted
   secrets, property test, error-path test.
5. Secrets: SecretBackend interface, Key Vault and envfile backends, TTL cache, 401 refetch.
6. Tenants: TenantSource from Key Vault tags, refresh loop, single-key default. Tests.
7. UniFi client: port HTTP, retry, pagination, host/site registry, per-tenant key and caps.
8. Catalog: YAML schema, loader, validator, first read operations (fleet, sites,
   devices, clients, networks, firewall policies, WLANs).
9. Read tools: list_tenants, search_operations, describe_operation, run_read,
   fleet_summary. Dev auth mode bound to 127.0.0.1 only.
10. Entra resource server: JWT validation, tid allowlist, scope, app roles, tenant groups.
11. OAuth facade: protected resource and AS metadata, /register, /authorize, /token proxy.
12. Changes: ChangeStore (SQLite, Azure Table), propose_change, get_change, expiry.
13. Approval backend: MSAL sign-in, sessions, CSRF, role and four-eyes rules. Tests.
14. Design: /impeccable init (PRODUCT.md), then DESIGN.md. No code.
15. Approval page UI: pending list, change detail with diff, approve and reject. I review.
16. Commit: commit_change, drift check, post-read, audit events, kill switches, rate
   limits. First low and high write operations in the catalog.
17. Secret-setting operations: server-generated or Key Vault sourced values only.
18. Webhook notifier for approvals, Teams and Slack examples in docs.
19. Azure infra: Bicep + azure.yaml, managed identity, Key Vault, Table, Container App.
20. Scripts: setup-entra, import-keys, smoke-test (bash and PowerShell).
21. Release: GHCR multi-arch image, SBOM, cosign signing, tag-driven workflow.
22. Live lab: deploy to my Azure, connect Copilot Studio, run the test matrix. I drive.
23. Docs: deploy-azure, entra-app-registration, copilot-studio, security, catalog, tenants.
24. README (written by me).

## Done

Nothing yet.

## Open decisions

- Typed tool params vs params_json string: decide after the task 22 Copilot Studio test.
- Self-approval for high risk: default on, revisit after task 22.

## Known issues

None yet.
