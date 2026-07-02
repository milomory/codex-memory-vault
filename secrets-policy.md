# Secrets Policy

- Date: 2026-06-30
- Source: user instruction and existing vault rules.
- Confidence: confirmed.

Never store these in the memory vault:

- passwords
- API keys
- private tokens
- SSH private keys
- recovery codes
- root/provider credentials
- raw `.env` contents
- private personal contact/payment data

Allowed instead:

- where a secret is stored, if the path itself is safe to mention;
- which command checks auth without printing credentials;
- whether a login is confirmed, stale, or needs re-checking;
- non-secret public endpoints, hostnames, and localhost/VPN ports.

When in doubt, write the operational fact and omit the secret value.
