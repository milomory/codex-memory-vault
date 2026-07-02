# Hyperion Memory Vault Ingest

- Date: 2026-07-02
- Source: live Hyperion `memory-core` ingest.
- Confidence: confirmed.

After creating the Athena GitLab mirror and pushing commit `d816e8d`, synced the
Mac `memory-vault` to Hyperion:

- Hyperion path: `/home/mil/Documents/Codex/memory-vault`
- Git state: `d816e8d` (`memory: sync shared infra baseline`)
- Legacy offload preserved outside the repo at:
  `/home/mil/Documents/Codex/memory-vault-legacy-offload-2026-07-02`

Created a pre-ingest backup:

- `/home/mil/.local/share/memory-core/backups/pre-vault-ingest-2026-07-02.sqlite`

Ingested into Hyperion `memory-core`:

- `project_id`: `project:codex-memory-vault`
- container path: `/memory-vault`
- indexed files: 23
- indexed chunks: 30

Verified hybrid search for `Athena GitLab memory vault deploy key`; expected
hits include `notes/athena-memory-vault-gitlab-2026-07-02.md` and
`infra/athena.md`.

The live service still uses SQLite at
`/home/mil/.local/share/memory-core/memory-core.sqlite`; PostgreSQL remains an
imported safety copy for now.
