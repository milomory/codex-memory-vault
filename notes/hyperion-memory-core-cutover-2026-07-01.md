# Hyperion memory-core cutover (2026-07-01)

## Summary

The old `codex-vds` memory work has been moved off the closing VDS path.
Hyperion (`mil@igorjan94.ru`, hostname `giperion`) is now the live host for the
shared `memory-core` API and the imported PostgreSQL safety copy.

## Live services on Hyperion

- `mc-memory-core`
  - Docker image: `memory-core-runtime:local`
  - Bind: `127.0.0.1:8765`
  - Data: `/home/mil/.local/share/memory-core/memory-core.sqlite`
  - API checks passed:
    - `/stats`
    - `/embeddings/status`
    - `/ui`
  - Last verified counts:
    - `nodes`: 151
    - `edges`: 200
    - `chunks`: 93
    - `embeddings`: 163
    - `events`: 1394
    - `project_tokens`: 5
    - `suggestions`: 1
  - Embeddings:
    - requested provider: `google`
    - active provider: `google_gemini`
    - model: `gemini-embedding-001@768`
    - missing chunks at cutover: 23

- `oc-gw-test`
  - Docker image: `node:22-slim`
  - Bind: `127.0.0.1:18790` on the host.
  - OpenClaw config inside Docker uses `gateway.bind=lan`; this is required so
    Docker bridge networking can reach the gateway. Host exposure remains
    loopback-only because Docker publishes the port to `127.0.0.1`.
  - API checks passed:
    - `/` returned `200`
    - `/health` returned `200`
  - Known warning: Telegram had transient network timeouts to `api.telegram.org`
    during verification. This is not a missing migration artifact.

- `pg-memory-core`
  - Docker image: `postgres:16`
  - Bind: `127.0.0.1:3588`
  - DB: `memory_core`
  - Status: legacy SQLite snapshot imported and row counts checked.
  - Important: current `memory-core` code is SQLite-only, so PostgreSQL is a
    safety copy / future backend target, not the live API backend yet.

## Supervisor decision

The `memory-core.service` and `openclaw-gateway.service` user systemd units were
disabled. They were tied to SSH user-session lifecycle and kept stopping Docker
containers on SSH logout via `ExecStop`.

Docker now owns these containers directly via `--restart unless-stopped`.
This keeps them running after SSH disconnect and lets Docker restart them after
daemon/server restart.

## Local-only policy

The service ports are local-only on Hyperion:

- `127.0.0.1:8765` - `memory-core`
- `127.0.0.1:18790` - OpenClaw gateway
- `127.0.0.1:3588` - PostgreSQL copy

Use SSH tunnels or a future VPN path for access. Do not expose these ports
directly to the public internet.

## Backups and rollback notes

- `memory-core` container switch backup:
  `/home/mil/.local/share/memory-core/backups/container-switch-20260701-135415`
- OpenClaw container/config switch backup:
  `/home/mil/.openclaw/.migration-backups/docker-bind-switch-20260701-135841`
- Local vault carries copied legacy artifacts under:
  `legacy-offload/`

Temporary pre-switch containers were removed after verification to recover disk.
Disk after cleanup was about `4.2G` free on `/` (`93%` used).

## Known follow-ups

- Keep an eye on Hyperion disk pressure; this host is small.
- Move OpenClaw plaintext gateway auth to SecretRefs later.
- Repair OpenClaw doctor warnings separately:
  - plugin drift for `@openclaw/codex`,
  - stale session metadata,
  - missing `message` tool in the Telegram-routed main agent.
- Implement a real PostgreSQL backend in `memory-core` only when the code is
  ready; do not assume the current API uses Postgres.
