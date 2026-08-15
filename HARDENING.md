<!-- markdownlint-disable -->

# Hardening Report: amondnet--vercel-action/v42.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **amondnet--vercel-action/v42.1.0** was hardened automatically. 9 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): Direct expression interpolation in run: blocks. In ci.yml, the 'Validate PR title' step interpolates `${{ github.event.pull_request.title }}` directly into a shell command (`echo "${{ github.event.pull_request.title }}" | pnpm exec commitlint`). A PR title containing shell metacharacters (e.g. `$(cmd)`, backticks, `;`) would be executed by the shell. The 'Validate commits' step also interpolates `${{ github.event.pull_request.base.sha }}` and `${{ github.event.pull_request.head.sha }}` directly into the shell command line without quoting.

Locations:

- `.github/workflows/ci.yml:30`
- `.github/workflows/ci.yml:34`

### script-injection (severity: high)

Sub-rule (a): Direct expression interpolation in run: block. In deploy.yml, the 'preview-url' step interpolates `${{ steps.vercel-action.outputs.preview-url }}` directly into an unquoted `echo` command (`echo ${{ steps.vercel-action.outputs.preview-url }}`). Step outputs are workflow-controllable and can contain shell metacharacters that would be interpreted by the shell.

Locations:

- `.github/workflows/deploy.yml:22`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` keys on any job. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially write) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/ci.yml:1`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` keys on any job. This workflow uses `pull_request_target` (which runs with write permissions by default) making the missing explicit permissions especially risky.

Locations:

- `.github/workflows/deploy.yml:1`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` keys on any job. This workflow uses `pull_request_target` trigger, making missing explicit permissions especially risky.

Locations:

- `.github/workflows/example-angular.yml:1`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` keys on any job. This workflow uses `pull_request_target` trigger, making missing explicit permissions especially risky.

Locations:

- `.github/workflows/example-express-basic-auth.yml:1`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` keys on any job. This workflow uses `pull_request_target` trigger, making missing explicit permissions especially risky.

Locations:

- `.github/workflows/example-nextjs.yml:1`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` keys on any job. This workflow uses `pull_request_target` trigger, making missing explicit permissions especially risky.

Locations:

- `.github/workflows/example-scope.yml:1`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` keys on any job. This workflow uses `pull_request_target` trigger, making missing explicit permissions especially risky.

Locations:

- `.github/workflows/example-static.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, missing-permissions

**Notes:**

Fixed script injection in ci.yml by moving PR title, base SHA, and head SHA expressions into env: blocks (PR_TITLE, BASE_SHA, HEAD_SHA) and referencing them as plain shell variables. Fixed script injection in deploy.yml by moving the preview-url step output into an env: block (PREVIEW_URL) and properly double-quoting it in the echo command. Added `permissions: contents: read` top-level blocks to all 7 workflow files that were missing permissions: ci.yml, deploy.yml, example-angular.yml, example-express-basic-auth.yml, example-nextjs.yml, example-scope.yml, and example-static.yml.

