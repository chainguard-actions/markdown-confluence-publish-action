<!-- markdownlint-disable -->

# Hardening Report: markdown-confluence--publish-action/v5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **markdown-confluence--publish-action/v5** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The 'tag major and minor versions' step in release-please.yml directly interpolates ${{ ... }} expressions inside a run: shell script (sub-rule a). Specifically, `${{ secrets.GITHUB_TOKEN }}`, `${{ steps.release.outputs.major }}`, and `${{ steps.release.outputs.minor }}` are embedded directly in shell commands such as `git remote add gh-token "https://${{ secrets.GITHUB_TOKEN }}@github.com/..."`, `git tag -d v${{ steps.release.outputs.major }}`, etc. These expressions are substituted by the YAML template engine before the shell ever sees them, meaning any special characters in the values are parsed by the shell without quoting. All ${{ ... }} expressions inside run: blocks are a script-injection risk regardless of their source.

Locations:

- `.github/workflows/release-please.yml:28`

### broad-permissions (severity: medium)

scorecard.yml has a top-level `permissions: read-all` declaration, which grants the GITHUB_TOKEN read access to all available scopes. This is overly broad and should be replaced with specific minimal permissions (e.g., `contents: read`, `security-events: write`, `id-token: write`) matching only what the workflow actually requires.

Locations:

- `.github/workflows/scorecard.yml:20`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, broad-permissions

**Notes:**

1. release-please.yml (script-injection): Moved ${{ secrets.GITHUB_TOKEN }}, ${{ steps.release.outputs.major }}, and ${{ steps.release.outputs.minor }} out of the run: shell block into an env: block as GITHUB_TOKEN, MAJOR, and MINOR. The shell script now references these as ${GITHUB_TOKEN}, ${MAJOR}, ${MINOR} with proper double-quoting throughout, eliminating the template-engine substitution before shell execution. 2. scorecard.yml (broad-permissions): Replaced the top-level `permissions: read-all` with specific minimal permissions `permissions: contents: read`. The job-level permissions block already specifies the write permissions needed (security-events: write, id-token: write), so the top-level default only needs contents: read for the checkout step.

