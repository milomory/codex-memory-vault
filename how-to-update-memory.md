# How To Update Memory

- Date: 2026-06-30
- Source: current shared-vault plan.
- Confidence: confirmed.

Use this vault as the reviewable source of durable context. Keep updates small,
plain-text, and git-reviewable.

## Before Work

For infrastructure, VDS, Codex, OpenClaw, OpenCode, or GitLab tasks, read:

- `AGENTS.md`
- `TODO.md`
- `infra/remote-codex-hosts.md`
- the relevant host file in `infra/`
- `secrets-policy.md`

For 42/WEBLIB tasks, also read:

- `projects/42.md`
- relevant project/task notes

## After Durable Changes

Add or update one small note with:

- `Date`
- `Source`
- `Confidence`
- current operational facts
- next action, if any

Do not mix unrelated topics in one update. Do not commit memory updates together
with unrelated code changes unless the user explicitly asks.

## Sync Shape

Preferred locations:

- Mac: `/Users/mil/Documents/Codex/memory-vault`
- Remote servers: `/home/anton/Documents/Codex/memory-vault`

The vault should be synced through git. Future server-side mirrors should keep
the same file layout and avoid storing secrets.

## Common Knowledge Pattern (Graph + Vectors)

- Treat this vault (`/Users/mil/Documents/Codex/memory-vault`) as the durable
  first source of truth.
- Use shared knowledge indexing (`memory-core`) for cross-project search, manifesting,
  relation graph, and history of durable decisions.
- Keep only operational facts in index access notes:
  - service/API endpoint,
  - service paths and ports,
  - token reference location (never token values),
  - backup locations and restore state.
- If shared-memory access is unavailable, continue from local docs and mark the block.
