# Knowledge Graph + Vector Rollout

- Date: 2026-07-01
- Source: OpenClaw `main` design docs + `memory-core` VDS handoff.
- Confidence: confirmed.

## Target state

- Build a shared graph-backed memory plane for all projects.
- Keep source repos on disk as source-of-truth.
- Keep `memory-vault` as durable, reviewable, git-versioned context for every chat.
- Keep `memory-core` as operational index for:
  - text search and semantic search,
  - project manifests,
  - explicit relations (`depends_on`, `implements`, `related_to`, `blocked_by`, etc.),
  - durable suggestions and events.

## Minimum viable local architecture

- Local file vault (`memory-vault`) + per-project `.memory-core.yml`.
- Shared service with:
  - project-scoped tokens,
  - immutable provenance links to file paths/lines,
  - suggestion inbox with approve/reject,
  - optional embeddings with local fallback.
  - prototype high-quality mode via `MEMORY_CORE_EMBEDDING_PROVIDER=google` +
    `google_gemini` (`gemini-embedding-001`), confirmed on `codex-vds`.
- Service path pattern:
  - `project_id: project:<logical-id>`
  - `api_base_url: http://127.0.0.1:8765`
  - `token_env: MEMORY_CORE_TOKEN`

## Current constraints

- Bridge-launched Codex sessions can block direct localhost service calls.
- Some server-side sessions still need server-side memory injection into prompts.
- `codex-vds` was local to that host; continuity is now handled by Hyperion.
- The current `memory-core` implementation is SQLite-only. PostgreSQL on
  Hyperion contains an imported safety copy, not the live API backend.

## Next implementation steps

1. Restore/initialize a local laptop memory-core runtime when needed for hands-on search.
2. Connect remaining clean projects via `connect-project` and `.memory-core.yml` with logical IDs.
3. Add/update project-level instructions:
   - check shared memory before wide changes,
   - write short event/decision records on durable actions,
   - continue from local docs if service is unavailable.
4. Use graph edges for cross-project routing:
   - `configured_shared_memory_for`,
   - `related_to`,
   - `depends_on`,
   - `supersedes` (where relevant).

## Imported from decommissioning `codex-vds` (2026-07-01)

- Transfer targets and snapshots pulled from:
  - `/home/anton/Documents/OpenClaw/projects/main` (OpenClaw memory product docs and rollout notes),
  - `/home/anton/.config/systemd/user/memory-core.service`,
  - `/home/anton/.local/share/memory-core/memory-core.sqlite` and backups,
  - `.memory-core.yml` files and live rollout state from both `main` and `memory-core` workspaces.
- A compact state snapshot with service/data-state and bridge limitations is stored at:
  - `notes/codex-vds-memory-core-snapshot-2026-07-01.md`.
- If this VDS becomes unavailable, this continuation plan is now pinned in:
  - `decisions/2026-07-01-memory-core-vds-offboarding.md`
  - `notes/memory-core-vds-transfer-2026-07-01.md`
  - `projects/codex-setup.md`

## Hyperion cutover context (active)

- Live host: `hyperion` (`mil@igorjan94.ru`, hostname `giperion`).
- Live API: `http://127.0.0.1:8765` on Hyperion, container
  `mc-memory-core`, image `memory-core-runtime:local`.
- Live data: `/home/mil/.local/share/memory-core/memory-core.sqlite`.
- Imported PostgreSQL copy: `pg-memory-core`, `127.0.0.1:3588`, DB
  `memory_core`.
- OpenClaw gateway: `oc-gw-test`, `127.0.0.1:18790`, `gateway.bind=lan`
  inside Docker but host-loopback-only externally.
- Detailed cutover note:
  - `notes/hyperion-memory-core-cutover-2026-07-01.md`

## Current shared-vault index status

- Date: 2026-07-02
- Source: live Hyperion `memory-core` ingest.
- Confidence: confirmed.

`memory-vault` was pushed to:

- Athena GitLab: `ssh://athena-gitlab/codex/memory-vault.git`
- GitHub: `git@github.com:milomory/codex-memory-vault.git`

Hyperion `memory-core` ingested the vault as:

- `project_id`: `project:codex-memory-vault`
- container path: `/memory-vault`
- indexed files after first stable-path ingest: 23
- indexed chunks after first stable-path ingest: 30
- indexed files after follow-up reingest: 24
- indexed chunks after follow-up reingest: 31
- observed stats after follow-up reingest: 175 nodes, 224 edges, 124 chunks,
  194 embeddings, 1631 events, 5 project tokens.

Search check:

- query: `Athena GitLab memory vault deploy key`
- expected hit: `notes/athena-memory-vault-gitlab-2026-07-02.md`
