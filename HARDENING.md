<!-- markdownlint-disable -->

# Hardening Report: amondnet--vercel-action/v42.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **amondnet--vercel-action/v42.2.0** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): ${{ }} expressions are interpolated directly inside run: shell commands. In ci.yml, the attacker-controlled PR title is injected: `echo "${{ github.event.pull_request.title }}" | pnpm exec commitlint`. Also in the same file: `pnpm exec commitlint --from ${{ github.event.pull_request.base.sha }} --to ${{ github.event.pull_request.head.sha }} --verbose`. These values come from pull request metadata and can be controlled by an attacker to inject shell commands.

Locations:

- `.github/workflows/ci.yml:30`
- `.github/workflows/ci.yml:34`

### script-injection (severity: high)

Rule (a): ${{ }} expressions are interpolated directly inside run: shell commands in deploy.yml. The secrets token is embedded inline: `vercel pull --yes --environment=preview --token=${{ secrets.VERCEL_TOKEN }}` and `vercel build --token=${{ secrets.VERCEL_TOKEN }}`. Additionally, a step output is echoed directly: `echo ${{ steps.vercel-action.outputs.preview-url }}`. Any ${{ }} expression in a run: block is a script-injection risk as the value is substituted before the shell parses the command.

Locations:

- `.github/workflows/deploy.yml:14`
- `.github/workflows/deploy.yml:18`
- `.github/workflows/deploy.yml:30`

### missing-permissions (severity: medium)

These workflow files have no top-level `permissions:` key and no job-level `permissions:` blocks on any of their jobs, meaning they run with the default (potentially broad) GITHUB_TOKEN permissions.

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

Fixed script injection in ci.yml by moving PR title and commit SHAs into env blocks (PR_TITLE, BASE_SHA, HEAD_SHA). Fixed script injection in deploy.yml by moving VERCEL_TOKEN into env blocks for the vercel CLI commands, and moving the preview-url step output into an env block (PREVIEW_URL). Added permissions blocks to all 7 workflow files: ci.yml gets 'contents: read'; deploy.yml and all 5 example workflows get 'contents: read' + 'pull-requests: write' since they use the vercel action with github-token to post PR comments.

