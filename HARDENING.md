<!-- markdownlint-disable -->

# Hardening Report: usefulness--diffuse-action/v0.11.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **usefulness--diffuse-action/v0.11.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in both workflow files use mutable version tags (e.g. @v4, @v3) instead of full 40-character SHA commit hashes. This means a compromised upstream action could silently inject malicious code. Affected references include: actions/checkout@v4, actions/setup-java@v4, actions/download-artifact@v4, actions/upload-artifact@v4, peter-evans/find-comment@v3, peter-evans/create-or-update-comment@v4 (in main.yml); and actions/checkout@v4, actions/setup-java@v4, actions/upload-artifact@v4 (in after_merge.yml). None are pinned to a SHA digest.

Locations:

- `.github/workflows/after_merge.yml:10`
- `.github/workflows/after_merge.yml:14`
- `.github/workflows/after_merge.yml:27`
- `.github/workflows/main.yml:14`
- `.github/workflows/main.yml:18`
- `.github/workflows/main.yml:23`
- `.github/workflows/main.yml:27`
- `.github/workflows/main.yml:35`
- `.github/workflows/main.yml:42`

### missing-permissions (severity: medium)

Neither .github/workflows/main.yml nor .github/workflows/after_merge.yml defines a top-level `permissions:` key, and no individual job within either file defines its own `permissions:` block. Without explicit permissions, workflows run with the repository's default token permissions, which may be overly broad (e.g. write access to contents, pull-requests, etc.).

Locations:

- `.github/workflows/main.yml:1`
- `.github/workflows/after_merge.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files (.github/workflows/main.yml and .github/workflows/after_merge.yml):

1. unpinned-uses: Pinned all action references to full 40-char SHA digests with original tags as comments:
   - actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262
   - actions/setup-java@v4 → @c1e323688fd81a25caa38c78aa6df2d33d3e20d9
   - actions/download-artifact@v4 → @d3f86a106a0bac45b974a628896c90dbdf5c8093
   - actions/upload-artifact@v4 → @ea165f8d65b6e75b540449e92b4886f43607fa02
   - peter-evans/find-comment@v3 → @3eae4d37986fb5a8592848f6a574fdf654e61f9e
   - peter-evans/create-or-update-comment@v4 → @71345be0265236311c031f5c7866368bd1eff043

2. missing-permissions: Added top-level `permissions: {}` to both workflow files. Jobs that post PR comments (test-apk and test-apk-2 in main.yml) received `permissions: pull-requests: write` at the job level. All other jobs received `permissions: {}` for least privilege.

