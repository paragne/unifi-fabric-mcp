# Decisions

Append only. One line per decision, with the reason.

- 2026-10-09: New repo, not a GitHub fork of swkstudios/unifi-fabric-mcp-server. Different scope (multi-tenant, Copilot Studio, approvals); upstream MIT notice kept in THIRD_PARTY_NOTICES.md.
- 2026-10-09: Python + FastMCP. Matches upstream so its client, pagination, and registry code can be ported instead of rewritten.
- 2026-10-09: Streamable HTTP only, no stdio or SSE. Copilot Studio requires streamable HTTP and SSE is deprecated there.
- 2026-10-09: Eight meta tools over an operation catalog instead of one tool per endpoint. Upstream has 283 tools; Copilot Studio caps at 128 and recommends 25 to 30.
- 2026-10-09: Tenants discovered from Key Vault secret names and tags, no tenant config file. Adding a customer needs no redeploy and keeps tenant data out of a public repo.
- 2026-10-09: In-app Entra JWT validation plus an OAuth facade (RFC 9728, 8414, 7591). Entra has no dynamic client registration, and Copilot Studio Dynamic discovery needs it.
- 2026-10-09: Writes use propose, out-of-band human approval, commit with pre-state hash. Model-supplied confirm flags are not a control.
- 2026-10-09: Approval page served by the same container. One deployable, shares the change store and Entra config.
- 2026-10-09: Server-rendered Jinja2 for the approval page, no JS framework. Small attack surface, strict CSP, nothing to build.
- 2026-10-09: Redact secrets, GPS, recognition data, and stream URLs from every result. Copilot Studio transcripts persist in Dataverse.
- 2026-10-09: Secret-setting operations never accept the value from the model. Server generates or reads it from Key Vault and returns the secret name only.
- 2026-10-09: Cloud-specific code behind interfaces (secrets, tenants, change store, identity). Other clouds come later without a rewrite.
- 2026-10-09: License Apache-2.0. Patent grant, compatible with upstream MIT.
- 2026-10-09: Not a learning entry for the stack. The new part is the OAuth facade and Copilot Studio integration.
- 2026-10-09: Repo named unifi-fabric-mcp