# Remote Codex Hosts

- Date: 2026-07-01
- Source: user-provided post-cleanup prompt, local `~/.ssh/config`, and live SSH checks.
- Confidence: mixed; `codex-vds` and `takeout-vds` are confirmed, `codex-athena` login is user-confirmed.

Do not bind infrastructure names to WEBLIB. WEBLIB is one working project; these
servers are shared infrastructure.

## Canonical SSH Aliases

Use these names on ShkidMacBook-Air:

- `codex-vds` - older VDS, normal Codex work user.
- `codex-athena` - Athena, normal Codex work user.
- `athena-root` - Athena root/admin access only.
- `athena-gitlab` - private GitLab SSH on Athena through `ProxyJump codex-athena`; currently used for the `codex/memory-vault` deploy key.
- `takeout-vds` - lightweight utility VDS for Takeout/offload workflows, not a normal Codex context by default.
- `hyperion` - live shared `memory-core` host and PostgreSQL safety-copy host.

Avoid and clean stale names if they appear in Codex Desktop/mobile UI:

- `athena`
- `athena.local`
- `athena-anton`
- `codex-weblib-vds`
- `codex-weblib-athena`
- `root@103.137.248.86` as a working Codex context

## Hosts

### `codex-vds`

- IP: `38.54.84.49`
- SSH user: `anton`
- Hostname: `wzmwfduq.vm`
- Codex CLI: `0.141.0`
- Auth: live check on 2026-06-30 returned `Logged in using ChatGPT`.
- Limitation: `anton` has no sudo; root by current key is unavailable.
- Note: mobile may still show `wzmwfduq.vm` because the hostname has not been renamed.

### `hyperion`

- Host/alias: `giperion` (`mil@igorjan94.ru`)
- Hostname: `giperion`
- Purpose: live shared knowledge persistence and API host after `codex-vds`
  offboarding.
- Current memory services:
  - `mc-memory-core`: `127.0.0.1:8765`, live SQLite API.
  - `pg-memory-core`: `127.0.0.1:3588`, imported PostgreSQL safety copy.
  - `oc-gw-test`: `127.0.0.1:18790`, OpenClaw gateway.
- Runtime note: `memory-core.service` and `openclaw-gateway.service` user units
  are disabled because SSH-session lifecycle stopped the containers. Docker
  restart policy owns these containers now.
- Status: not yet used as Codex remote context.
- Note: keep service ports localhost/VPN-only. Use SSH tunnels or future VPN
  access, not public internet exposure.

### `takeout-vds`

- IP: `38.54.13.221`
- SSH user: `anton`
- Hostname: `hfdt6esc.vm`
- OS: `Ubuntu 24.04.2 LTS`
- Resources: `1` CPU, `1.9GiB` RAM, `50G` root disk with about `40G` free in live check on 2026-07-01.
- SSH key reference: local SSH config points to `~/.ssh/takeout_vds_38_54_13_221`; do not store key contents in the vault.
- Auth: key-based SSH live check succeeded on 2026-07-01.
- Purpose: separate utility host for Google Takeout/offload work. Keep it distinct from `codex-vds`, which is the older Codex/OpenClaw/OpenCode VDS.
- Codex state: do not assume Codex CLI/app-server is installed unless re-verified.

### `codex-athena`

- IP: `103.137.248.86`
- SSH user: `anton`
- Hostname: `athena.local`
- Codex CLI: `0.142.3`
- Auth: user reported successful `codex login --device-auth` and `codex login status` on 2026-06-28.
- UI policy: keep GitLab/OpenClaw/OpenCode localhost/VPN-only by default.
- GitLab memory mirror: `ssh://athena-gitlab/codex/memory-vault.git`.
- Note: live SSH from this Mac succeeded on 2026-07-02; re-verify reachability
  when provider maintenance or VPN routing has changed.

### `athena-root`

- IP: `103.137.248.86`
- SSH user: `root`
- Purpose: server administration only.
- Do not use as normal Codex context without explicit reason.
- Expected state: root-side Codex app-server was disabled so mobile should not show `root@103.137.248.86` as a normal working Codex server.

### `athena-gitlab`

- Purpose: Git over SSH into Athena GitLab without exposing GitLab publicly.
- SSH target: `git@127.0.0.1:2224` through `ProxyJump codex-athena`.
- Host key alias: `athena-gitlab`.
- Current project: `codex/memory-vault`.
- Local git remote in `/Users/mil/Documents/Codex/memory-vault`: `athena`.
- Deploy key reference: `~/.ssh/memory_vault_athena_gitlab_ed25519`; never
  store private key contents in the vault.

## Codex Desktop Expected State

- Working remote Codex servers: `codex-vds` and `codex-athena`.
- Utility SSH hosts: `takeout-vds`.
- Auto-connect should include normal work contexts only.
- Root/admin connections should stay out of auto-connect.
- If mobile/desktop shows stale names, clean the stale state instead of using those entries.

## Operational Note

- Track follow-up memory-core/Postgres backend work in `TODO.md` and
  `projects/knowledge-memory-core.md`.
