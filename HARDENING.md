<!-- markdownlint-disable -->

# Hardening Report: markdown-confluence--publish-action/v0.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **markdown-confluence--publish-action/v0.0.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The `run:` block in the 'tag major and minor versions' step directly interpolates GitHub Actions expressions inside shell commands. Specifically: `${{ secrets.GITHUB_TOKEN }}` is embedded in a git remote URL, and `${{ steps.release.outputs.major }}` / `${{ steps.release.outputs.minor }}` are used directly in git tag and push commands. Per the script-injection check, ANY `${{ ... }}` expression inside a `run:` shell command string is a violation (sub-rule a), as YAML template substitution occurs before the shell processes the string, allowing injection of shell metacharacters. Offending lines include:
- `git remote add gh-token "https://${{ secrets.GITHUB_TOKEN }}@github.com/..."`
- `git tag -d v${{ steps.release.outputs.major }} || true`
- `git tag -d v${{ steps.release.outputs.major }}.${{ steps.release.outputs.minor }} || true`
- (and several more git tag/push lines using the same expressions)
Fix: move these values into `env:` variables and reference them as `"$ENV_VAR"` in the shell script.

Locations:

- `.github/workflows/release-please.yml:33`

### broad-permissions (severity: medium)

The workflow file sets top-level `permissions: read-all`, which grants overly broad read access across all permission scopes. This should be replaced with specific minimal permissions required by each job.

Locations:

- `.github/workflows/scorecard.yml:18`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, broad-permissions

**Notes:**

1. release-please.yml: Moved ${{ secrets.GITHUB_TOKEN }}, ${{ steps.release.outputs.major }}, and ${{ steps.release.outputs.minor }} out of the run: shell block into an env: block as GITHUB_TOKEN, MAJOR, and MINOR respectively. The shell script now references these as plain environment variables with proper double-quoting, eliminating the script-injection risk. 2. scorecard.yml: Replaced the broad `permissions: read-all` top-level permission with specific `permissions: contents: read`, which is the minimal permission needed at the workflow level. The analysis job already has its own job-level permissions block with the specific permissions it needs (security-events: write, id-token: write).

