<!-- markdownlint-disable -->

# Hardening Report: markdown-confluence--publish-action/v0.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **markdown-confluence--publish-action/v0.0.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### invalid-yaml (severity: high)

action.yml is not valid YAML and will fail to load (act reports a "Failure - Main ./" before any test runs): yaml: line 14: did not find expected key. A common cause is a single-line run: whose value begins with a quote, e.g. `run: "$VAR" -m pip install ...` — YAML parses "$VAR" as a complete quoted scalar and then rejects the trailing words. Convert it to a block scalar so the whole command is literal:
  run: |
    "$VAR" -m pip install ...

Locations:

- `action.yml:14`

## Iteration Notes

### Iteration 1

**Fixes applied:** invalid-yaml

**Notes:**

Fixed invalid YAML in action.yml at line 14. The atlassianUserName description was a single-quoted YAML string containing an apostrophe in "account's", which broke YAML parsing. Fixed by escaping the apostrophe using YAML's single-quote escape syntax (doubling it: '' instead of ').

