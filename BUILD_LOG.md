# Build Log

This file is an **append-only, chronological record** of how this framework was built.
Every meaningful push gets a new entry below, in reverse-chronological order (newest on top).
The intent: any team member (or future you) can trace *why* something exists, not just *what* exists.

Each entry follows this format:

```
## [Phase N] Short Title — YYYY-MM-DD
**Branch:** branch-name
**Commit(s):** short-sha (or "see PR #x")

### What was done
- ...

### Why
- ...

### Notes / Decisions
- ...
```

---

## [Phase 0] Repository & Governance Foundation — 2026-08-21
**Branch:** `main` (initial commit, pre-GitFlow)
**Commit(s):** initial commit

### What was done
- Initialized local git repository, set default branch to `main`.
- Created the locked folder structure: `features/`, `config/`, `src/` (with
  `steps/`, `pages/`, `support/world/`, `support/hooks/`, `api/`, `utils/`),
  `data/`, `reports/`, `test-results/`, `.github/`.
- Added `.gitignore` covering: `node_modules`, all real `.env*` files (only
  `.env.example` is tracked), Playwright output (`test-results/`,
  `playwright-report/`), Allure output (`reports/allure-results`,
  `reports/allure-report`), storage-state session files, build output, logs,
  and IDE/OS cruft.
- Added `.gitattributes` to normalize line endings to LF, mark `.feature`
  files as Gherkin for GitHub linguist stats, and mark report directories
  as non-diffable.
- Added `.nvmrc` pinning Node `20.17.0` for consistent tooling across
  local/CI environments.
- Added `.env.example` as the single source of truth for expected
  environment variable names (base URL, saucedemo user credentials,
  execution settings, logging, reporting) — **no real values committed**.
- Created initial `README.md` (tech stack, project structure, setup
  instructions, placeholders for run commands/tag strategy/CI to be filled
  in as those phases land).
- Created this `BUILD_LOG.md`.

### Why
- Folder structure was finalized upfront (features/config at root, src/
  reserved for importable framework code) to avoid churn later — restructuring
  mid-build breaks step definition paths, tsconfig paths, and CI globs.
- `.gitignore`/`.env.example` were created **before** any real config, so
  there is no window in git history where a secret could have been committed
  and then "removed" (removal doesn't purge history).
- README and BUILD_LOG were scaffolded in the same first commit so every
  subsequent phase has a place to log into from day one, rather than
  retrofitting documentation after the framework exists.

### Notes / Decisions
- GitFlow branches (`develop`, `feature/*`, etc.) are created in the next
  step, branching off this initial `main` commit.
- Branch protection on `main`/`develop` will be applied via GitHub UI after
  `develop` exists (protection rules for `develop` need the branch to exist
  first).
- No dependencies installed yet — `package.json` and tooling land in Phase 1.
