---
status: historical
---

Repo: simeonfab/ResolveOS-intake-frontend

PRODUCTION BUG - confirmed via Vercel runtime logs, not a guess:

Every call to /api/resolve is failing with FUNCTION_INVOCATION_FAILED. The underlying
log shows: "Failed to load the ES module: /var/task/api/resolve.js" - api/resolve.js
uses ES module syntax (`export default async function handler...`), but package.json
does not declare `"type": "module"`, so Vercel's Node runtime is trying to load it as
CommonJS and failing before the function body ever executes. This is unrelated to the
OPENAI_API_KEY env var - the function fails before it gets that far.

FIX: Add `"type": "module"` to package.json.

After adding this, double check api/resolve.js doesn't use any CommonJS-only syntax
(e.g. require(), module.exports) anywhere - it should be pure ESM (import/export)
throughout, since the project will now be strictly treated as an ES module project.
If you find any CommonJS syntax remaining (for example in the fs file-reading logic
added for the resolveos-context/ file injection in the last round of changes - check
specifically whether that uses require('fs') or import fs from 'fs'), convert it to
ESM import syntax.

Also verify vercel.json (added in the last round of changes, for the
resolveos-context/** includeFiles config) doesn't contain anything that conflicts
with or needs adjusting for this module type change - check Vercel's current
documentation if uncertain.

After fixing: git add -A && git commit -m "Fix ES module loading failure - declare
type:module in package.json" && git push origin main. Confirm the push succeeded,
report the commit hash, and confirm you tested that api/resolve.js has no remaining
CommonJS syntax.
