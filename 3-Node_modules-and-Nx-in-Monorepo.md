# Node_modules and Nx in Monorepo - Learning Guide

## What is node_modules?
**node_modules** is where npm stores all external libraries your project needs.

### Key Concepts:
- **Dependencies declared in**: `package.json`
- **Dependencies stored in**: `node_modules/` folder
- **Size**: Can be 100MB-1GB+ with thousands of files
- **Contains**: Direct dependencies + their dependencies (dependency tree)

### Import Flow:
```typescript
// You write this:
import { Component } from '@angular/core';

// It comes from:
node_modules/@angular/core/
```

### Best Practices:
- ✅ Add `node_modules/` to `.gitignore`
- ✅ Run `npm install` after cloning
- ❌ Never commit node_modules to git
- ❌ Never edit files inside node_modules

---

## What is Nx?
**Nx** is a build system for managing monorepos (multiple projects in one repository).

### Without Nx (Traditional):
```
my-projects/
├── main-app/           # Separate repo
│   ├── package.json
│   └── node_modules/   # 200MB
├── shared-ui/          # Separate repo  
│   ├── package.json
│   └── node_modules/   # 150MB
└── utilities/          # Separate repo
    ├── package.json
    └── node_modules/   # 180MB
```
**Problems**: 3 repos, 3 node_modules, version sync nightmare, complex code sharing

### With Nx (Current Setup):
```
frontend-monorepo/
├── apps/
│   └── main-application/    # Your main app
├── libs/
│   ├── shared-services/     # Shared utilities
│   └── ui-components/       # Reusable components
├── package.json             # Single package.json
└── node_modules/            # Single 300MB folder
```

### Nx Benefits:
- **Single repo** - everything together
- **Single node_modules** - shared dependencies
- **Instant code sharing** - no publishing needed
- **Smart builds** - only rebuilds what changed
- **Integrated testing** - test everything together

### Common Nx Commands:
```bash
nx build main-application        # Build app
nx test ui-components           # Test library
nx affected:test                # Test only changed code
nx graph                        # View dependency graph
```

### Daily Workflow Example:
**Task**: Add new button component and use it

**Without Nx**: 10 steps involving multiple repos, publishing, version management
**With Nx**: 
1. `nx generate component button --project=ui-components`
2. Export from library
3. Import in app: `import { ButtonComponent } from '@company/ui-components'`
4. Use immediately - everything just works!

---

## Summary
- **node_modules**: Your project's "pantry" of external libraries
- **Nx**: Makes managing multiple related projects easy in one repo
- **Together**: Nx + single node_modules = efficient, maintainable development
