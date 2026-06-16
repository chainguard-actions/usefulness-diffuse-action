<!-- markdownlint-disable -->

# Hardening Report: usefulness--diffuse-action/v0.8.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **usefulness--diffuse-action/v0.8.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The `run:` block in action.yml directly interpolates a `${{ ... }}` expression inside the shell command string. Specifically, `${{ github.action_path }}` is embedded directly in the shell command: `run: python3 '${{ github.action_path }}'/entrypoint.py`. Although `github.action_path` is typically GitHub-controlled, any `${{ ... }}` expression inside a `run:` block undergoes YAML template substitution before the shell processes it, making it a script-injection risk. The safe pattern is to use the `$GITHUB_ACTION_PATH` environment variable instead.

Locations:

- `action.yml:97`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Replaced `${{ github.action_path }}` in the `run:` block with the `$GITHUB_ACTION_PATH` environment variable. Changed `run: python3 '${{ github.action_path }}'/entrypoint.py` to `run: python3 "$GITHUB_ACTION_PATH/entrypoint.py"`. This eliminates the YAML template substitution risk while preserving identical runtime behavior, since `$GITHUB_ACTION_PATH` is always set by GitHub Actions to the same value as `github.action_path`.

