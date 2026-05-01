---
name: setup-pre-commit
description: Set up Lefthook pre-commit hooks with lint-staged (Biome), type checking, and tests in the current repo. Use when user wants to add pre-commit hooks, set up Husky, configure lint-staged, or add commit-time formatting/typechecking/testing.
---

# Setup Pre-Commit Hooks

## What This Sets Up

- **Lefthook** pre-commit hook
- **lint-staged** running Prettier on all staged files
- **Prettier** config (if missing)
- **typecheck** and **test** scripts in the pre-commit hook

## Steps

### 1. Detect package manager

Check for `package-lock.json` (npm), `pnpm-lock.yaml` (pnpm), `yarn.lock` (yarn), `bun.lockb` (bun). Use whichever is present. Default to npm if unclear.

### 2. Install dependencies

Install as devDependencies:

```
lefthook
```

Add install pinnded devDependency:

```
@biomejs/biome -E
```

### 3. Setup Lefthook

Run the following:

```bash
lefthook install
```

This creates an empty `lefthook.yml` for configuration.

### 4. Update `lefthook.yml`

Write this file

```yaml
pre-commit:
  commands:
    lint-format:
      glob: "**/*.{js,ts,jsx,tsx,mjs,cjs,json,css}"
      run: ./node_modules/.bin/biome check --write {staged_files}
      stage_fixed: true
```

### 5. Setup Biome

```bash
npx @biomejs/biome init
```

**Adapt**: Replace npx with detected package manager (e.g. `pnpx`, `bunx --bun `).

This creates a default biome config file.

### 6. Verify

- [ ] `lefthook.yml` exists
- [ ] `biome.json` exists
- [ ] Run `lefthook validate` to verify it works

### 7. Commit

Stage all changed/created files and commit with message: `Add pre-commit hooks (lefthook + biome)`

This will run through the new pre-commit hooks — a good smoke test that everything works.

## Notes

- `LEFTHOOK=0 git commit` skips running lefthook rules.
- The pre-commit runs lint-staged first (fast, staged-only), then full typecheck and tests
