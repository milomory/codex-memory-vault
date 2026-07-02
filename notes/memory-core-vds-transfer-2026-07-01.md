# memory-core transfer notes (2026-07-01)

- `codex-vds` still exposes a working local-first memory service at `127.0.0.1:8765`.
- The knowledge design from OpenClaw is now mirrored to this vault.

## What was running on VDS

- `memory-core` project: `/home/anton/Documents/OpenClaw/projects/memory-core`
- Service unit: `/home/anton/.config/systemd/user/memory-core.service`
- Data store:
  - `~/.local/share/memory-core/memory-core.sqlite`
  - `~/.local/share/memory-core/backups/`
- API: FastAPI + web UI + `memoryctl` + graph/edge/event/suggestion endpoints.
- Project contract pattern: `.memory-core.yml` with `project_id`, `api_base_url`, `token_env`, `root`, include/exclude globs.
- Tokens: project-scoped in env, no token values in repository/vault.

## VDS-hosted OpenClaw design that should be preserved

- Treat source code and secrets as file system source of truth.
- Use memory service for:
  - searchable references and summaries,
  - typed relations between projects,
  - events and decision write-back,
  - suggestion inbox with approval.
- Keep embeddings as local baseline (`hash`) with provider abstraction for upgraded modes.
- Use `project:`-style logical ids for stable cross-host stability.

## Transfer actions already captured here

- Added decision note: `decisions/2026-07-01-memory-core-vds-offboarding.md`
- Updated:
  - `AGENTS.md`
  - `README.md`
  - `how-to-update-memory.md`
  - `TODO.md`
  - `projects/codex-setup.md`
- Added stable reference for bridge-era limits:
  - laptop-bridge Codex sandbox can be network-isolated,
  - so server-side OpenClaw must inject memory context into prompts when needed.
