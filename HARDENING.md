<!-- markdownlint-disable -->

# Hardening Report: markdown-confluence--publish-action/v0.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **markdown-confluence--publish-action/v0.0.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The `run:` block in the 'tag major and minor versions' step directly interpolates GitHub Actions expressions inside shell commands (rule a). Multiple `${{ steps.release.outputs.major }}`, `${{ steps.release.outputs.minor }}`, and `${{ secrets.GITHUB_TOKEN }}` expressions are embedded directly in shell command strings (e.g., `git tag -d v${{ steps.release.outputs.major }} || true`, `git remote add gh-token "https://${{ secrets.GITHUB_TOKEN }}@github.com/...`). Any expression interpolated directly into a `run:` block is a script-injection risk because the value is substituted into the shell command string before the shell parses it, allowing metacharacters to be interpreted. These should be moved to `env:` variables and referenced as quoted shell variables (e.g., `"$MAJOR"`).

Locations:

- `.github/workflows/release-please.yml:30`

### broad-permissions (severity: medium)

The workflow file sets `permissions: read-all` at the top level (line 18). This grants overly broad read access across all permission scopes and should be replaced with specific minimal permissions (e.g., `contents: read`, `security-events: write`, `id-token: write`) scoped only to what the workflow actually needs.

Locations:

- `.github/workflows/scorecard.yml:18`

### invalid-yaml (severity: high)

action.yml is not valid YAML and will fail to load (act reports a "Failure - Main ./" before any test runs): yaml: line 14: did not find expected key. A common cause is a single-line run: whose value begins with a quote, e.g. `run: "$VAR" -m pip install ...` — YAML parses "$VAR" as a complete quoted scalar and then rejects the trailing words. Convert it to a block scalar so the whole command is literal:
  run: |
    "$VAR" -m pip install ...

Locations:

- `action.yml:14`

## Iteration Notes

### Iteration 1

**Fixes applied:** invalid-yaml, script-injection, broad-permissions

**Notes:**

1. action.yml (invalid-yaml): Fixed YAML parsing error on line 14 where the atlassianUserName description was wrapped in single quotes but contained an apostrophe (account's). Changed to double-quoted string. 2. .github/workflows/release-please.yml (script-injection): Moved ${{ secrets.GITHUB_TOKEN }}, ${{ steps.release.outputs.major }}, and ${{ steps.release.outputs.minor }} out of the run: shell block into an env: block, referencing them as ${GITHUB_TOKEN}, ${MAJOR}, and ${MINOR} shell variables with proper quoting. 3. .github/workflows/scorecard.yml (broad-permissions): Replaced top-level 'permissions: read-all' with specific 'permissions: contents: read', which is the minimal permission needed at the workflow level (the job already specifies its own security-events: write and id-token: write permissions).

