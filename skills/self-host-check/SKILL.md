---
name: self-host-check
description: Verify a self-hosted open-source Kortix install before the team relies on it.
---

# Self-host check for open-source Kortix

Run this before the team depends on a self-hosted open-source Kortix instance. Work through the four checks and report the exact command output, not a summary.

## 1. CLI and stack

```bash
kortix version
kortix self-host status
kortix self-host doctor
```

The version prints, every service is running, and the doctor reports no failed check.

## 2. Reachability

- The dashboard answers on the configured domain over HTTPS.
- `api.<domain>` answers.
- Start a session with `kortix sessions new --prompt "health check" --wait`. A session that boots proves the sandbox provider is reachable from the box, which is the one dependency that lives off the box.

## 3. Data and secrets

- Back up all three of these under `~/.config/kortix/self-host/<instance>/`: `volumes/db/data`, `volumes/storage` and `.env`.
- Open the model picker and confirm the team's provider keys are connected. Self-hosted instances use the team's own keys by default.

## 4. Update channel

- Read the current channel: `stable` (default) or `latest`.
- To pin a version, run `kortix self-host update --tag <version>`.
- Leave automatic updates on unless the team has a reason to freeze.

## Report

Return one short block: version, stack status, session result, backup paths, channel. Flag anything that failed with the raw output, and do not mark the install ready until all four checks pass.
