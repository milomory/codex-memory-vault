# Codex Setup

## Status

- Date: 2026-06-17
- Confidence: confirmed

The local Codex setup includes a broad skill pack with marketing, frontend,
security, deployment, research, document, and workflow skills.

`define-goal` was installed from the official OpenAI skills catalog.

## Current Direction

Build a more structured Codex operating system:

- use explicit modes like audit, plan, implementation, research, and debug;
- keep durable memory in files;
- sync memory to GitHub for review and continuity;
- install third-party skills only after source review.

## VDS Codex Remote

- Date: 2026-06-23
- Source: live VDS setup and Codex App mobile/desktop checks
- Confidence: confirmed

The VDS Codex setup was rebuilt under the remote user `anton` with the
standalone Codex CLI. The active CLI is resolved through
`/home/anton/.local/bin/codex`, and `codex doctor` reported a healthy logged-in
state with WebSocket connectivity and a persistent app server.

For phone access, the important server-side mode is remote control, started
with:

```bash
codex remote-control start
```

The working mobile connection showed up as `CLI wzmwfduq.vm · Connected`.
The older `leo-vds` entry stayed `Offline` and should be treated as stale; it
can be removed from the mobile app.

Remote Codex project folders should live under:

```bash
/home/anton/Documents/Codex/projects
```

SSH from the laptop is confirmed working through the local SSH alias:

```bash
mil@ShkidMacBook-Air ~ % ssh codex-vds
```

## VDS OpenCode Agent

- Date: 2026-06-26
- Source: live VDS install and web/CLI checks
- Confidence: confirmed

OpenCode is installed on the VDS under the remote user `anton`.

```bash
/home/anton/.opencode/bin/opencode
```

The verified version was `1.17.11`. OpenCode has its own OpenAI OAuth
credential and is separate from Codex CLI/Codex App; it does not run through
`codex remote-control`.

Check auth and available models:

```bash
ssh codex-vds
opencode auth list
opencode models
```

The confirmed auth state was `OpenAI oauth` with `1 credentials`, and OpenAI
models such as `openai/gpt-5.5` and `openai/gpt-5.5-fast` were available.

Run OpenCode interactively inside a project:

```bash
ssh codex-vds
cd /home/anton/Documents/Codex/projects/<project>
opencode -m openai/gpt-5.5
```

Run a one-shot agent task:

```bash
ssh codex-vds
cd /home/anton/Documents/Codex/projects/<project>
opencode run -m openai/gpt-5.5-fast "Describe the task here"
```

OpenCode also has a browser UI. Use it through an SSH tunnel instead of
exposing it publicly.

Start the server on the VDS from the project directory:

```bash
ssh codex-vds
cd /home/anton/Documents/Codex/projects/<project>
opencode serve --hostname 127.0.0.1 --port 4096
```

From the Mac, create a tunnel:

```bash
ssh -fN -o ExitOnForwardFailure=yes -L 4096:127.0.0.1:4096 codex-vds
```

Then open:

```text
http://127.0.0.1:4096
```

Do not bind OpenCode to `0.0.0.0` or expose it directly to the public internet
unless authentication, HTTPS, and access controls are deliberately configured.
The server warned that it is unsecured when no `OPENCODE_SERVER_PASSWORD` is
set.

If OpenCode needs to be authorized again, use the headless ChatGPT login:

```bash
opencode auth login -p openai -m "ChatGPT Pro/Plus (headless)"
```

## Next Actions

- Decide GitHub destination for the memory vault.
- Review third-party skills from the Reddit/Habr-inspired workflow discussion.
- Consider creating a local `workflow-router` skill.

## Common Knowledge Graph + Vector Service (Recovery State)

- Date: 2026-07-01
- Source: `codex-vds` remote diagnostics and OpenClaw `main` project knowledge docs.
- Confidence: confirmed.

OpenClaw-backed `memory-core` existed on the old VDS and became the shared
knowledge core for projects, but this VDS is being phased out.

Known operating pattern to keep:

- File-first truth for source code in projects.
- Shared memory service for references/chunks/edges/events and cross-project hints.
- `.memory-core.yml` contract in each project (`project_id`, `api_base_url`,
  `token_env`, `root`, `include/exclude`).
- Project-scoped tokens for API read/write.
- No project secrets in service DB/content.

Critical design that should be preserved:

- SQLite canonical store for local-first mode.
- FTS + vector search (hash fallback, with provider abstraction).
- Typed graph edges (`depends_on`, `implements`, `related_to`, etc.).
- Suggestion inbox with approve/reject workflow.

Discovered legacy VDS facts to preserve on transition:

- Service unit path: `/home/anton/.config/systemd/user/memory-core.service`.
- DB path: `/home/anton/.local/share/memory-core/memory-core.sqlite` with backups in
  `/home/anton/.local/share/memory-core/backups/`.
- Service API: `http://127.0.0.1:8765`.
- Last known status: service enabled and running on `codex-vds`.
- OpenClaw/OpenClaw `main` project used logical ID: `project:openclaw-main`.

Next continuation pattern (when `memory-core` is available):

- Use `memoryctl` for `connect-project`, `ingest-project`, `search`, `suggest`.
- For bridge-driven Codex work, fetch context server-side and inject into prompt when
  sandbox blocks direct service calls.
- For normal laptop work, use local `http://127.0.0.1:8765` directly with proper token context.
