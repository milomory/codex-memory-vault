# Takeout VDS

- Date: 2026-07-01
- Source: local `~/.ssh/config` and live SSH checks.
- Confidence: confirmed.

This is a lightweight utility VDS for Google Takeout/offload workflows. Keep it
separate from the older Codex VDS (`codex-vds`) and from Athena.

## Identity

- SSH alias: `takeout-vds`
- Public IP: `38.54.13.221`
- SSH user: `anton`
- Hostname: `hfdt6esc.vm`
- OS: `Ubuntu 24.04.2 LTS`
- Kernel: `6.8.0-62-generic`
- SSH key reference: local SSH config uses `~/.ssh/takeout_vds_38_54_13_221`; never store the private key contents here.

## Resources

Live check on 2026-07-01:

- CPU: `1`
- RAM: `1.9GiB`
- Root filesystem: `50G` total, about `40G` available

## Purpose

- Use for Takeout/offload and transfer-related utility work.
- Do not treat this as a normal Codex Desktop remote context unless Codex is
  explicitly installed and verified later.
- Keep secrets, SSH private keys, OAuth tokens, and cloud credentials out of the
  vault; store only references to where access material is configured.
