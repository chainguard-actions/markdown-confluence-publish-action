<!-- markdownlint-disable -->

# Hardening Report: markdown-confluence--publish-action/v7.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **markdown-confluence--publish-action/v7.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### broad-permissions (severity: medium)

The workflow file .github/workflows/scorecard.yml sets `permissions: read-all` at the top level. This grants overly broad read access to all GitHub Actions scopes and should be replaced with specific minimal permissions (e.g., `contents: read`, `security-events: write`, `id-token: write`) rather than the blanket `read-all` grant.

Locations:

- `.github/workflows/scorecard.yml:18`

## Iteration Notes

### Iteration 1

**Fixes applied:** broad-permissions

**Notes:**

Replaced the top-level `permissions: read-all` in `.github/workflows/scorecard.yml` with specific minimal permissions (`contents: read`). The job-level permissions block already specifies `security-events: write` and `id-token: write` for the specific needs of the Scorecard analysis job, so the top-level only needs `contents: read` for the checkout step. This eliminates the overly broad `read-all` grant while preserving all required functionality.

