<!-- markdownlint-disable -->

# Hardening Report: amondnet--vercel-action/v42.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **amondnet--vercel-action/v42.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Direct GitHub Actions expression interpolation inside run: shell commands. In ci.yml, the PR title (attacker-controlled) is interpolated directly: `echo "${{ github.event.pull_request.title }}" | pnpm exec commitlint` (rule a). Also, commit SHAs are interpolated directly: `pnpm exec commitlint --from ${{ github.event.pull_request.base.sha }} --to ${{ github.event.pull_request.head.sha }} --verbose` (rule a). In deploy.yml, a step output is interpolated directly: `echo ${{ steps.vercel-action.outputs.preview-url }}` (rule a). All of these allow shell metacharacter injection before the shell ever sees the value.

Locations:

- `.github/workflows/ci.yml:31`
- `.github/workflows/ci.yml:36`
- `.github/workflows/deploy.yml:22`

### missing-permissions (severity: medium)

These workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any job, meaning they run with the default (potentially broad) token permissions granted by the repository settings.

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/deploy.yml:1`
- `.github/workflows/example-angular.yml:1`
- `.github/workflows/example-express-basic-auth.yml:1`
- `.github/workflows/example-nextjs.yml:1`
- `.github/workflows/example-scope.yml:1`
- `.github/workflows/example-static.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, missing-permissions

**Notes:**

Fixed script-injection in ci.yml by moving PR title and commit SHA expressions into env: blocks (PR_TITLE, BASE_SHA, HEAD_SHA). Fixed script-injection in deploy.yml by moving the step output expression into an env: block (PREVIEW_URL) and double-quoting it. Added top-level permissions blocks to all 7 workflow files: ci.yml gets 'contents: read'; deploy.yml and all 5 example workflows get 'contents: read' + 'pull-requests: write' since they use the vercel action which posts PR comments.

