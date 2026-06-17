# Reusable Workflows

## Long Task Startup

1. Clarify the goal and success criteria.
2. Read relevant memory files.
3. Inspect the current repo or environment before changing anything.
4. Make a short plan only when the task is broad or risky.
5. Execute, verify, then summarize what changed.
6. Update memory if durable context was learned.

## Memory Update

1. Capture only durable facts, decisions, preferences, and open loops.
2. Put project state in `projects/`.
3. Put decisions in `decisions/`.
4. Put next actions in `TODO.md`.
5. Run `git diff -- codex-memory-vault` for review.

## Third-Party Skill Intake

1. Read the repository README.
2. Inspect each target `SKILL.md`.
3. Check install scripts for shell writes, network calls, and destructive
   behavior.
4. Install only the skills that fill a real workflow gap.
5. Record installed skills and rationale in memory.
