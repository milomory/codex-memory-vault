# Hyperion Shared Memory Host

- Date: 2026-07-01
- Source: direct SSH check `mil@igorjan94.ru`
- Confidence: confirmed

## Identity

- Hostname: `giperion`
- SSH user: `mil`
- Alias used in requests: `hyperion`

## Current role

Hyperion is now the live host for the shared `memory-core` API after the old
`codex-vds` offboarding.

Live API:

- Container: `mc-memory-core`
- Image: `memory-core-runtime:local`
- Bind: `127.0.0.1:8765`
- Live store: `/home/mil/.local/share/memory-core/memory-core.sqlite`
- Runtime owner: Docker restart policy (`unless-stopped`), not user systemd.

OpenClaw gateway on the same host:

- Container: `oc-gw-test`
- Bind: `127.0.0.1:18790`
- Config detail: `gateway.bind=lan` inside Docker, with the Docker published
  port restricted to host loopback.

VPN-only web domains added on 2026-07-01:

- `monitor.vpn` -> `82.146.44.70` -> Hyperion nginx -> `127.0.0.1:8099`
- `memory.vpn` / `memory-core.vpn` -> `82.146.44.70` -> Hyperion nginx -> `127.0.0.1:8765`
- `openclaw.vpn` -> `82.146.44.70` -> Hyperion nginx -> `127.0.0.1:18790`
- `xcontest.vpn` -> `82.146.44.70` -> Hyperion nginx -> `127.0.0.1:8766`

DNS for these names is served by the VPN entrypoint
`amnezia-cabinet-dns.service` drop-in
`/etc/systemd/system/amnezia-cabinet-dns.service.d/vpn-services.conf` on the
Moscow, Germany, and Dallas gateways. Hyperion nginx allows only those VPN
egress IPs plus localhost; direct public `Host: *.vpn` requests should return
`403`.

PostgreSQL is present as an imported safety copy / future backend target. The
current `memory-core` code path is SQLite-only.

## Dedicated memory-core DB container (created)

- Container: `pg-memory-core`
- Image: `postgres:16`
- Exposed DB endpoint: `127.0.0.1:3588` -> `5432`
- Database: `memory_core`
- User: `memory_core`
- Access path: via local host port forwarding or server-local service config.
- Status: bootstrapped and loaded from legacy SQLite snapshot (counts checked, no import errors).

Imported counts:

- `nodes`: 151
- `edges`: 200
- `chunks`: 93
- `embeddings`: 163
- `events`: 1394
- `suggestions`: 1
- `project_tokens`: 5

## Observed State

- `psql` client is not present in `PATH` for `mil` shell.
- `postgres` processes are running, and PostgreSQL is provided via Docker containers.
- Host OS identified as `Linux giperion` (kernel `4.15.0-66-generic` branch from `uname -a`).
- PostgreSQL containers detected:
  - `pg-tink-robot`:
    - image `postgres` (PG 13)
    - local DB: `tink`
    - owner user: `<redacted service user>`
    - exposed host port: `3579`
    - data mount: `/home/mil/postgre` -> `/var/lib/postgresql/data`
  - `pg-saas`:
    - image `postgres` (PG 13)
    - local DBs: `postgres`, `saas`
    - owner user: `saas`
    - exposed host port: `5229`
    - data mount: docker anonymous volume
  - `pg-crypto-robot`:
    - image `postgres:16-alpine`
    - local DB: `robot_crypto`
    - owner user: `crypto_robot`
    - exposed host port: `3580` (bound to `127.0.0.1`)
    - data mount: docker volume `robotcryptojsnode_pg_crypto_robot_data`

## Why it matters

This host is the current durable target for `memory-core` service continuity
after `codex-vds` is retired.

## Remaining actions

- Keep the live API on SQLite until the code has a real PostgreSQL storage backend.
- Use the imported PostgreSQL DB as a safety copy and migration target.
- Monitor disk pressure; after cleanup `/` still had only about `4.2G` free.
- Keep all operational secrets out of vault; store them in secure host-local env/config only.

## Security notes

- Do not store DB passwords/client credentials in `memory-vault`.
- Record only safe operational facts (host, process presence, migration steps).
