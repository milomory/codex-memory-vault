# Open Loops

## Active

- [ ] Push the reviewed `memory-vault` baseline to the Athena GitLab remote
  after the current dirty docs are committed.
- [ ] Review and optionally install third-party skills from
  `reachmeshailesh-boop/codex-skill-pack`.
- [ ] Migrate the `memory-core` graph-vector service design from the decommissioning VDS to a laptop-first operation path:
  - install/run memory-core in a separate laptop workspace,
  - keep `.memory-core.yml` + stable project IDs everywhere,
  - use this note plus decisions as the transfer handoff source.
- [ ] Add a real PostgreSQL storage backend to `memory-core` when needed.
  Hyperion already has an imported `pg-memory-core` safety copy, but the live
  API still uses SQLite.
- [ ] Clean up OpenClaw doctor warnings on Hyperion:
  - move plaintext gateway auth to SecretRefs,
  - repair `@openclaw/codex` plugin drift,
  - review stale session metadata,
  - add `message`/messaging tool access for the Telegram-routed main agent if
    Telegram replies need explicit channel actions.
- [ ] Monitor Hyperion disk pressure; after service cutover cleanup `/` was
  still around 93% used.
- [ ] Capture required Hyperion DB access facts in one safe vault note:
  - psql binary path,
  - cluster version,
  - database name/owner/schema plan.

## Waiting

- [ ] Choose which workflows deserve custom local skills.

## Done

- [x] Installed `define-goal` from the official OpenAI skills catalog.
- [x] Moved the memory vault to `/Users/mil/Documents/Codex/memory-vault`.
- [x] Connected and pushed the memory vault to
  `git@github.com:milomory/codex-memory-vault.git`.
- [x] Created private Athena GitLab mirror project `codex/memory-vault` and
  added local remote `athena` via `ssh://athena-gitlab/codex/memory-vault.git`
  (`2026-07-02`).
- [x] Transferred VDS `memory-core` design details and shared-memory rollout artifacts into this vault for continuity (`2026-07-01`).
- [x] Cut over live `memory-core` API to Hyperion Docker runtime on
  `127.0.0.1:8765`; moved OpenClaw gateway to loopback-only host publishing on
  `127.0.0.1:18790`; disabled SSH-session-bound user services in favor of
  Docker restart policy (`2026-07-01`).
