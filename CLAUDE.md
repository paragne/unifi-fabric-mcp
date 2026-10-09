# unifi-msp-mcp

Open-source MCP server for the UniFi Site Manager / Fabric cloud API, built for MSPs
and designed for Microsoft Copilot Studio as the client. Many tenant API keys in
Azure Key Vault, Entra ID OAuth, human-approved writes, secrets never reach the chat.

@docs/STATE.md

## Stack

- Python 3.12, FastMCP (streamable-http, stateless), Starlette, httpx, pydantic-settings
- Auth: PyJWT + Entra JWKS for MCP tokens, MSAL auth code + PKCE for the approval page
- Azure: azure-identity, azure-keyvault-secrets, azure-data-tables
- Approval page: server-rendered Jinja2, plain CSS, no JS framework
- uv with uv.lock, every dependency pinned exact. pytest, respx, hypothesis, ruff, mypy --strict
- Container: python:3.12-slim multi-stage, non-root, port 8080. Infra: Bicep + azd
- License Apache-2.0. Upstream MIT attribution in THIRD_PARTY_NOTICES.md

## Commands

- `make dev` : run the server on the devbox at 127.0.0.1:8080 (I run it, not you)
- `make check` : ruff, ruff format --check, mypy, pytest, pip-audit. Must pass before every commit.
- `make test` : pytest only
- `make image` : docker build

## Layout

- `src/unifi_msp_mcp/server.py` : app wiring, /mcp, /healthz, OAuth and approval routes
- `config.py` : env parsing, fails fast at startup
- `auth/` : IdentityProvider interface, Entra validation, OAuth facade
- `secrets/` : SecretBackend interface (keyvault, envfile for dev only)
- `tenants/` : TenantSource, discovered from Key Vault secrets `unifi-key-*` and their tags
- `unifi/` : HTTP client, retry, pagination, host/site registry (ported from upstream)
- `catalog/operations.yaml` + loader : every callable UniFi operation with a risk tier
- `tools/` : the MCP tools (8 max)
- `changes/` : propose, approve, commit pipeline; ChangeStore (SQLite dev, Azure Table prod)
- `approvals/` : approval page routes and templates
- `redaction/`, `audit/`
- `infra/azure/`, `scripts/`, `docs/`, `tests/`
- `reference/upstream/` : gitignored clone of swkstudios/unifi-fabric-mcp-server. Read only.

## Security rules (non-negotiable)

- No UniFi key, Key Vault value, PSK, RADIUS secret, or token in tool args, tool
  results, errors, logs, audit records, fixtures, or commit messages.
- Every tool result and error passes through `redaction.sanitize`. No bypass.
- The model only sees tenant slugs. Keys resolve server side.
- No MCP tool can approve a change. `confirm` style arguments are not a control.
- Writes need: app role in the token, an approved change record, and a fresh
  pre-state hash match at commit. Critical changes need a second person.
- No generic write passthrough. Every write is a catalog operation with `pre_read`.
- `WRITES_ENABLED` defaults false. Per-tenant `writes` tag defaults off.
- Approval page: SameSite=Strict session cookie, CSRF token on every POST, strict
  CSP, frame-ancestors none, diff rendered from the stored record only.
- All fixtures are synthetic. No real tenant names, MACs, IPs, SSIDs, hostnames,
  Entra IDs, or subscription IDs anywhere in the repo.
- Never invent a UniFi endpoint. Every catalog entry has a `source` (upstream file or developer.ui.com URL).
- Ask before adding any dependency and say why.

## Code rules

- Files under 200 lines. Type hints everywhere, mypy strict clean.
- Azure-specific code only behind the interfaces in `secrets/`, `tenants/`, `changes/`, `auth/`.
- Mock api.ui.com with respx and Azure with fakes. Tests never hit a real service.
- If a test fails, diagnose first. Say whether code or test is wrong and wait for my call before changing a test.
- Files derived from upstream carry the attribution header from THIRD_PARTY_NOTICES.md.

## UI rules

- The only UI is the approval page. Read PRODUCT.md and DESIGN.md before touching it.
- Works on a phone (approvals come from a Teams or Slack link).
- UI work is not done until I have seen it running. Stop and tell me what to check.

## Session rules

- One task per session, from docs/STATE.md.
- Small commits to main, conventional commit messages, no Co-Authored-By trailer. Never push.
- Append architecture decisions to docs/DECISIONS.md, one line each with the reason.
- End every session with /handoff.
