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

## Next Actions

- Decide GitHub destination for the memory vault.
- Review third-party skills from the Reddit/Habr-inspired workflow discussion.
- Consider creating a local `workflow-router` skill.
