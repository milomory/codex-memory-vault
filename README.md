# Codex Memory Vault

Durable working memory for Codex: projects, decisions, preferences, and open
loops that should survive individual chats.

The goal is not to record everything. The goal is to keep useful context in
plain files that can be reviewed, edited, committed, and reused.

## GitHub Setup

This vault is intended to live in a private GitHub repository:

```bash
cd /Users/mil/Documents/Codex/memory-vault
git remote -v
git push -u origin main
```

Keep it private unless every note inside is safe to publish.

## Daily Use

Start broad work by asking Codex to read:

```text
Use codex-memory-vault as shared memory. Read AGENTS.md, TODO.md, and any
relevant project notes before starting. Update memory when you learn durable
facts or close loops.
```

Before finishing a session, ask:

```text
Update the memory vault with durable decisions, open loops, and project status.
Show me the diff before committing.
```
