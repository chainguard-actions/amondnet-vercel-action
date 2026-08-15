<!-- markdownlint-disable -->

# Hardening Report: amondnet--vercel-action/v42.2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **amondnet--vercel-action/v42.2.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Direct expression interpolation of attacker-controlled `github.*` context values inside `run:` shell commands. (a) Line 29: `echo "${{ github.event.pull_request.title }}"` — a PR title is fully attacker-controlled and is interpolated directly into the shell command, enabling command injection. (b) Line 34: `pnpm exec commitlint --from ${{ github.event.pull_request.base.sha }} --to ${{ github.event.pull_request.head.sha }}` — SHA values from the PR event are interpolated directly into the shell command. These should be passed via `env:` variables and referenced as `"$VAR"` in the shell.

Locations:

- `.github/workflows/ci.yml:29`
- `.github/workflows/ci.yml:34`

### script-injection (severity: high)

Direct expression interpolation of `steps.*.outputs.*` context inside a `run:` shell command. Line 32: `echo ${{ steps.vercel-action.outputs.preview-url }}` — the step output value is interpolated directly into the shell command without quoting or env-var indirection. A malicious deployment URL could contain shell metacharacters. This should be passed via an `env:` variable and referenced as `"$VAR"` in the shell.

Locations:

- `.github/workflows/deploy.yml:32`

### missing-permissions (severity: medium)

Workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g., write access to contents and pull-requests). Explicit minimal permissions should be declared.

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

Fixed 3 script-injection instances and added permissions blocks to 7 workflow files. (1) ci.yml line 29: moved github.event.pull_request.title to env var PR_TITLE. (2) ci.yml line 34: moved github.event.pull_request.base.sha and head.sha to env vars BASE_SHA/HEAD_SHA. (3) deploy.yml line 32: moved steps.vercel-action.outputs.preview-url to env var PREVIEW_URL. Added top-level permissions blocks: ci.yml gets 'contents: read'; deploy.yml and all 5 example workflows get 'contents: read' + 'pull-requests: write' since they use github-token to post PR deployment comments.

