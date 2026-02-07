---
name: monorepo-imports
description: Enforces correct cross-package imports in monorepos — use the public API, never reach into source files
---

# Monorepo Imports

In a monorepo, every package has a public API defined by its `exports` field in `package.json`. Cross-package imports MUST go through that API — never reach into another package's source files.

## Rules

### Always import from the package name
```typescript
// CORRECT — uses the public API
import { createModule } from '@scope/core';
import { Trie } from '@scope/core/internals';

// WRONG — reaches into source files
import { createModule } from '@scope/core/src/module';
import { Trie } from '@scope/core/src/router/trie';
```

### The `exports` field is the contract
A package's `exports` in `package.json` defines what other packages can import. If something isn't exported, it's private — don't import it directly from source.

```json
{
  "exports": {
    ".": { "import": "./dist/index.js" },
    "./internals": { "import": "./dist/internals.js" }
  }
}
```

This means consumers can import from `@scope/core` and `@scope/core/internals` — nothing else.

### Use subpath exports for internal APIs
When an official package (e.g., a testing library) needs internal APIs that shouldn't be public:
1. Create a subpath like `./internals` with its own entry point
2. Export it in `package.json`
3. Import from `@scope/core/internals`, not from source paths

### No aliases that bypass the package boundary
Do NOT add vitest aliases, tsconfig paths, or bundler aliases that redirect `@scope/pkg` to source files. Let workspace resolution use the built `dist/` output. This ensures:
- Tests run against the same code consumers get
- Missing exports are caught immediately
- Package boundaries are enforced, not just documented

### When something is missing from the public API
If you need something that isn't exported:
1. Check if it belongs in the public API — if yes, export it from the package's index
2. Check if it belongs in an internal subpath — if yes, export it from `./internals`
3. If neither, reconsider the dependency — you may need to restructure
