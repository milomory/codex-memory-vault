# memory-core Google Gemini Prototype (codex-vds)

- Context: `codex-vds` (`38.54.84.49`, `anton`) before shutdown.
- Date: 2026-07-01 (checked before host decommissioning path).

## What was real in the live state

- Service file: `/home/anton/.config/systemd/user/memory-core.service`.
- Service sets `MEMORY_CORE_EMBEDDING_PROVIDER=hash` by default.
- Service also loads `EnvironmentFile=-/home/anton/.config/memory-core/env`.
- That environment file contained variables:
  - `MEMORY_CORE_EMBEDDING_PROVIDER=google`
  - `MEMORY_CORE_GOOGLE_EMBEDDING_MODEL=gemini-embedding-001`
  - `MEMORY_CORE_GOOGLE_EMBEDDING_DIMENSION=768`
  - key variable for Google API (names used by implementation included
    `MEMORY_CORE_GOOGLE_API_KEY` and `GEMINI_API_KEY`, value redacted)
- Because environment file is loaded after service defaults, effective runtime provider was **Google Gemini**.
- Runtime check from `/embeddings/status` reported:
  - `requested: google`
  - `active: google_gemini`
  - `model: gemini-embedding-001@768`
  - `dimension: 768`
  - `key_configured: true`

## Project/docs evidence from VDS

- `memory-core/AGENTS.md` explicitly documented a two-mode strategy:
  - hash as local lightweight default,
  - Google Gemini as higher-quality prototype path.
- `memory-core/README.md` and `docs/PROJECT_BOUNDARY.md` also described a prototype path with `MEMORY_CORE_EMBEDDING_PROVIDER=google` and `gemini-embedding-001`.
- `.memory-core.yml` files used tokenized API integration, stable `project_id`, and `http://127.0.0.1:8765`.

## Historical note

- This should be treated as a prototype path (`prototype`) due to higher external dependency and resource profile compared with pure local hash embeddings.
- For continuity, keep in mind that the canonical durable state is in this vault and repository snapshots, not token-bearing files.
