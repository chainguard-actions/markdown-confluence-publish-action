<!-- markdownlint-disable -->

# Hardening Report: markdown-confluence--publish-action/v6.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **markdown-confluence--publish-action/v6.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### broad-permissions (severity: medium)

scorecard.yml sets top-level `permissions: read-all`, which grants overly broad read access across all permission scopes. This should be replaced with specific minimal permissions (e.g., `contents: read`, `security-events: write`, `id-token: write`) scoped to only what each job requires.

Locations:

- `.github/workflows/scorecard.yml:17`

## Iteration Notes

### Iteration 1

**Fixes applied:** broad-permissions

**Notes:**

Replaced top-level `permissions: read-all` in .github/workflows/scorecard.yml with specific minimal permissions (`contents: read`). The job-level permissions block already defines the specific permissions needed (`security-events: write` for uploading code scanning results, `id-token: write` for publishing results/badge). The top-level `contents: read` provides the minimal baseline needed for the checkout step without granting broad read access across all permission scopes.

