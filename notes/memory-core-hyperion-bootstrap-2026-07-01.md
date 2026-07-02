# Hyperion memory-core PostgreSQL bootstrap (2026-07-01)

## What was done

- Created dedicated PostgreSQL container on Hyperion:
  - container: `pg-memory-core`
  - image: `postgres:16`
  - bind: `127.0.0.1:3588 -> 5432`
- Created database/user:
  - DB: `memory_core`
  - user: `memory_core`
- Imported legacy `memory-core-offboard-2026-07-01.sqlite` snapshot into PostgreSQL:
  - `nodes` — 151
  - `chunks` — 93
  - `edges` — 200
  - `embeddings` — 163
  - `events` — 1394
  - `suggestions` — 1
  - `project_tokens` — 5
  - `chunk_fts` (compatibility) — 93

## Verification commands run

- `docker exec -e PGPASSWORD=... pg-memory-core psql -U memory_core -d memory_core -c "SELECT version();"`
- Count checks against all imported tables (all matched expected)

## Notes

- This note intentionally does not store credentials.
- Keep credentials in server-local secure storage when wiring service configs.
- Next step: connect memory-core service to this DB and validate API search/query behavior.
