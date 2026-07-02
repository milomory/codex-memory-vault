# Codex Memory Vault

This repository stores durable working memory for Codex sessions. Treat it as a
reviewable notebook, not as an invisible memory dump.

## Rules

- Never store secrets, API keys, passwords, private tokens, recovery codes, full
  personal documents, payment details, or private contact details.
- Prefer short, factual notes over broad impressions.
- Every memory update should be useful in a future task and easy to review in a
  git diff.
- If a note is uncertain, mark it as `Unconfirmed`.
- When closing tasks or making decisions, update the relevant project or
  decision file.
- Do not overwrite user-written notes without preserving their intent.

## What To Update

- `TODO.md`: active open loops, follow-ups, and waiting-for items.
- `projects/`: durable state for projects, products, repositories, and systems.
- `people/`: working preferences and collaboration context, only when useful.
- `decisions/`: decisions with date, context, options, and rationale.
- `notes/`: daily or ad hoc notes that do not yet belong elsewhere.
- `agent/preferences.md`: stable preferences for working with Codex.
- `agent/workflows.md`: repeatable workflows that should be reused.

## Memory Update Format

When updating memory, keep entries compact:

- Date: `YYYY-MM-DD`
- Source: where the information came from
- Confidence: `confirmed`, `inferred`, or `unconfirmed`
- Action: what should happen next, if any

## Review Habit

Memory changes should be reviewed like code:

```bash
git diff -- codex-memory-vault
```

Commit memory updates separately from unrelated code changes when possible.

## Shared memory-core

- Protect private memory and secrets: do not print, store, or expose secrets,
  tokens, credentials, private config, or private memory contents.
- When shared memory-core is available, check/search it for
  `project:codex-memory-vault` before work that may depend on shared project
  context.
- In case the service is unavailable or VDS-hosted memory services are retired,
  continue from local docs and explicitly add a short recovery note to this vault.
- After meaningful changes, write short shared memory summaries when
  appropriate.
- If memory-core is unavailable, say so explicitly and continue with local
  repository context. The same applies when bridge restrictions prevent direct
  access from launch contexts.
- Local or normal Mac Codex sessions can use `http://127.0.0.1:8765` directly.
  Bridge-driven Codex may instead receive memory context injected by OpenClaw,
  because the bridge sandbox has no direct network access.
