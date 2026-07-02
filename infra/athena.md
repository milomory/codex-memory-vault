# Athena

- Date: 2026-06-30
- Source: bootstrap note plus user-provided post-cleanup prompt.
- Confidence: confirmed for setup baseline, user-confirmed for latest Codex login.

`athena.local` is the newer private infrastructure server for GitLab and agent
tooling. It is shared infrastructure, not a WEBLIB-specific server.

## Identity

- SSH alias for normal work: `codex-athena`
- SSH alias for admin: `athena-root`
- Public IP: `103.137.248.86`
- Normal user: `anton`
- Hostname: `athena.local`
- OS baseline: Ubuntu 24.04.4 LTS
- Resources observed at setup: 4 CPU, 7.7 GiB RAM, 3.8 GiB swap, 79 GiB root disk

## Project Roots

- Codex: `/home/anton/Documents/Codex/projects`
- OpenClaw: `/home/anton/Documents/OpenClaw/projects`
- OpenCode: `/home/anton/Documents/OpenCode/projects`
- GitLab data/config/logs: `/opt/gitlab`

## Services

- Codex CLI: `0.142.3`
- Codex app-server: installed as standalone managed daemon for `anton`
- OpenClaw: `127.0.0.1:18790`
- OpenCode: `127.0.0.1:4096`
- GitLab CE container: `athena-gitlab`
- GitLab HTTP: `127.0.0.1:8088`
- GitLab SSH: `127.0.0.1:2224`

All web UIs should remain localhost/VPN-only unless there is an explicit
security decision to expose them another way.

## GitLab Memory Mirror

- Date: 2026-07-02
- Source: live setup from ShkidMacBook-Air via `athena-root`.
- Confidence: confirmed.

Athena GitLab has a private project for the shared memory vault:

- GitLab path: `codex/memory-vault`
- Local git remote name: `athena`
- Remote URL from the Mac: `ssh://athena-gitlab/codex/memory-vault.git`
- SSH alias: `athena-gitlab`
- Access path: `ProxyJump codex-athena` to GitLab SSH on Athena
  `127.0.0.1:2224`
- Deploy key reference: `~/.ssh/memory_vault_athena_gitlab_ed25519`

The deploy key is project-scoped for `codex/memory-vault`; do not copy the
private key into this vault. The GitLab SSH host key is stored locally under the
host key alias `athena-gitlab`.

## Temporary Access Before VPN

From the Mac:

```bash
ssh -N -L 18088:127.0.0.1:8088 -L 14096:127.0.0.1:4096 -L 18792:127.0.0.1:18790 codex-athena
```

Then open:

- GitLab: `http://127.0.0.1:18088`
- OpenCode: `http://127.0.0.1:14096`
- OpenClaw: `http://127.0.0.1:18792`

## Current Notes

- User completed Codex device login from phone on 2026-06-28 and reported `codex login status` as logged in.
- Root-side Codex should not be used as a working context.
- When VPN/local DNS is ready, change GitLab `external_url` away from localhost to the VPN-local name.
