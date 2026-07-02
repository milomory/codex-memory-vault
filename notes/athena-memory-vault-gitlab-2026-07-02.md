# Athena Memory Vault GitLab Mirror

- Date: 2026-07-02
- Source: live Athena GitLab setup from ShkidMacBook-Air.
- Confidence: confirmed.

Created a private Athena GitLab project for the shared memory vault:

- Project: `codex/memory-vault`
- Mac remote: `athena`
- Remote URL: `ssh://athena-gitlab/codex/memory-vault.git`
- Access: `athena-gitlab` SSH alias, using `ProxyJump codex-athena` to reach
  GitLab SSH on Athena `127.0.0.1:2224`.
- Deploy key reference:
  `~/.ssh/memory_vault_athena_gitlab_ed25519`

Verified:

- `ssh -T athena-gitlab` returns the GitLab greeting.
- `git ls-remote ssh://athena-gitlab/codex/memory-vault.git` succeeds against
  the empty project.

Do not store the deploy private key, GitLab root password, or provider
credentials in this vault.
