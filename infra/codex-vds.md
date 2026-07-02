# Older Codex VDS

- Date: 2026-06-30
- Source: local SSH config, live SSH check, and prior VDS setup notes.
- Confidence: confirmed.

This is the older VDS used for Codex/OpenClaw/OpenCode and miscellaneous remote
work. It is shared infrastructure, not a WEBLIB-specific server.

## Identity

- SSH alias: `codex-vds`
- Public IP: `38.54.84.49`
- SSH user: `anton`
- Hostname: `wzmwfduq.vm`
- Codex CLI: `0.141.0`
- Auth: `Logged in using ChatGPT` in live check on 2026-06-30.

## Project Roots

- Codex: `/home/anton/Documents/Codex/projects`
- OpenClaw: `/home/anton/Documents/OpenClaw/projects`
- OpenCode: `/home/anton/Documents/OpenCode/projects`

## Known Limits

- `anton` currently has no sudo.
- Root access by the current key is unavailable.
- Hostname has not been renamed, so some UIs may still show `wzmwfduq.vm`.

## Working Principle

Use this host for normal remote project work through `codex-vds`. Do not create
WEBLIB-specific host aliases for shared infrastructure.
