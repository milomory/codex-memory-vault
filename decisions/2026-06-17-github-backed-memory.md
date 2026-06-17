# GitHub-Backed Codex Memory

- Date: 2026-06-17
- Status: proposed
- Confidence: confirmed

## Context

A Habr article about getting more from Codex described using long-lived threads
plus a file-based memory vault, often stored in GitHub, so agent memory becomes
visible, editable, and reviewable.

## Decision

Adopt a file-based memory vault for durable context and prepare it for syncing
to a private GitHub repository.

## Rationale

- Git diffs make memory updates reviewable.
- Files survive individual Codex threads.
- Multiple Codex threads can read the same shared context.
- Bad or vague memories can be edited or reverted.

## Guardrails

- Keep the repository private by default.
- Do not store secrets, tokens, private documents, or sensitive contact details.
- Ask for review before committing large memory changes.
