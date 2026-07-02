# Decision: shared memory transfer from closing VDS

- Date: 2026-07-01
- Source: live reads from `codex-vds`, OpenClaw `main` docs, and service state inspection.
- Confidence: confirmed.

## Decision

- The current shared memory design (`memory-core`) was developed and run on the old VDS
  (`codex-vds`) as an independent knowledge service, not as OpenClaw-owned storage.
- Because that VDS is being closed, the laptop now keeps the canonical operational
  knowledge in this vault and uses it as the handoff source for all future chats.
- `memory-core` should be brought up as a local-first shared service again when needed,
  without any dependency on that old host for normal workflow continuity.

## Preserved facts from VDS offboarding

- Service: `/home/anton/.config/systemd/user/memory-core.service`
  - type simple
  - user-unit enabled
  - working directory `/home/anton/Documents/OpenClaw/projects/memory-core`
  - `uvicorn` on `127.0.0.1:8765`
  - default embedding provider in VDS state: `hash`
  - runtime embedding provider from active env override: `google_gemini` (`gemini-embedding-001@768`) while `GEMINI_API_KEY` was present
  - optional env file `~/.config/memory-core/env`
- Service health (last checked): enabled and active/running.
- Database: `~/.local/share/memory-core/memory-core.sqlite`
- Backups: `~/.local/share/memory-core/backups/`
- Active projects in VDS memory graph included `project:openclaw-main` and laptop-mapped
  nodes for `project:*` under codex-vds inventory flows.
- Main docs present in `~/Documents/OpenClaw/projects/main`:
  - `PROJECT_GRAPH_ARCHITECTURE.md`
  - `PROJECT_MEMORY_SYSTEM_SPEC.md`
  - `OPENCLAW_PROJECT_WORKFLOW.md`
  - `LAPTOP_PROJECTS_INTEGRATION.md`
  - `CODEX_PROJECT_INVENTORY_PROMPT.md`
  - `VPN_PROJECT_TAKEOVER.md`
- OpenClaw note: `main` project `.memory-core.yml` used logical id `project:openclaw-main`.

## Immediate continuation plan on laptop

1. Keep this file and `projects/codex-setup.md` as the canonical handoff record.
2. Extend `AGENTS.md` and project instructions to fail-safe on `memory-core` unavailability.
3. Rebuild or restore `memory-core` in a controlled laptop-backed environment if/when the old service is needed for graph/vector operations.
4. Continue all new project onboarding through file-safe `memory-core` contracts and per-project tokens.
