---
status: historical
---

Repo: simeonfab/ResolveOS-intake-frontend (clone or locate it locally if not already
present, on the main branch).

PRODUCTION BUG - confirmed via Vercel runtime logs, not a guess:

Every call to /api/resolve is failing with FUNCTION_INVOCATION_FAILED. The underlying
runtime log shows: "Failed to load the ES module: /var/task/api/resolve.js" -
api/resolve.js uses ES module syntax (`export default async function handler...`),
but package.json does not declare `"type": "module"`, so Vercel's Node runtime is
trying to load it as CommonJS and failing before the function body ever executes.
This is unrelated to the OPENAI_API_KEY env var - the function fails before it gets
that far.

STEPS:

1. Open package.json and add `"type": "module"` to it.

2. Open api/resolve.js and check it for any CommonJS-only syntax (require(),
   module.exports, etc.) - it must be pure ESM (import/export) throughout once the
   project is treated as an ES module project. Pay particular attention to the
   file-reading logic added in the last round of changes (used to inject the real
   resolveos-context/ markdown files into the API prompts) - check whether it uses
   `require('fs')` or `import fs from 'fs'` / `import { readFileSync } from 'fs'`,
   and convert to ESM import syntax if needed.

3. Check vercel.json (added in the last round of changes, for the
   resolveos-context/** includeFiles config) for anything that might conflict with
   or need adjusting for this module type change. If uncertain, check Vercel's
   current documentation on ES modules in serverless functions.

4. Run a local syntax/sanity check on both files after editing (e.g. `node --check
   api/resolve.js`) before committing.

5. Stage, commit, and push:
   git add -A
   git commit -m "Fix ES module loading failure - declare type:module in package.json"
   git push origin main

6. Confirm the push succeeded and report the commit hash. Confirm explicitly that you
   checked api/resolve.js and vercel.json and found (or fixed) any remaining
   CommonJS/module-type conflicts - don't just report the package.json change alone.

Do not modify anything else in the repo beyond what's needed to fix this specific
issue.
