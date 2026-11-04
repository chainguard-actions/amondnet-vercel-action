<!-- markdownlint-disable -->

# Hardening Report: amondnet--vercel-action/v42.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **amondnet--vercel-action/v42.3.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index 5a315fe..6569471 100644
--- a/action.yml
+++ b/action.yml
@@ -122,7 +122,6 @@ inputs:
     required: false
     description: 'Opt in to the experimental API-based deployment via @vercel/client. The CLI is the default and recommended path. The API client relies on an internal Vercel package without semver guarantees and may break across updates. Mutually exclusive with vercel-args (default: false).'
     default: 'false'
-
 outputs:
   preview-url:
     description: deployment preview URL
@@ -133,7 +132,7 @@ outputs:
 runs:
   using: node24
   main: dist/index.js
-
 branding:
   icon: triangle
   color: white

```

Locations:

- `action.yml`

## Iteration Notes

### Iteration 1

**Fixes applied:** prior-fix

**Notes:**

The earlier patch only deleted two blank lines in action.yml: one before 'outputs:' and one before 'branding:'. It made no security change. I deleted the same two lines again so the file matches the earlier patch. action.yml is a node24 action that runs dist/index.js. It has no run: steps, uses: references, permissions or expressions, so nothing else in it needed hardening.

