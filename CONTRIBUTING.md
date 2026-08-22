# Contributing Guide

This document defines how work flows through this repository: branching model,
naming conventions, commit style, and pull request rules. Everyone contributing
to this framework is expected to follow this — it is what keeps `BUILD_LOG.md`
and git history meaningful over time.

---

## Branching Strategy (GitFlow)

This repo follows **GitFlow**, extended with a `framework/*` branch type specific
to test-automation repos (for core framework changes that aren't tied to a single
feature's test coverage).

### Branch types

| Branch        | Branches off | Merges into        | Lifetime   | Purpose |
|---------------|--------------|---------------------|------------|---------|
| `main`        | —            | —                   | Permanent  | Production-ready. Every commit here is a tagged, releasable state. |
| `develop`     | `main`       | —                   | Permanent  | Integration branch. Always reflects the latest completed, tested work. |
| `feature/*`   | `develop`    | `develop`           | Temporary  | New test coverage or new capability tied to a story/ticket. |
| `framework/*` | `develop`    | `develop`           | Temporary  | Core framework infrastructure: hooks, fixtures, World, config, utils, page-object base classes — changes not scoped to one feature's tests. |
| `bugfix/*`    | `develop`    | `develop`           | Temporary  | Fixing a bug in test code or framework discovered in `develop` (unreleased). |
| `hotfix/*`    | `main`       | `main` **and** `develop` | Temporary | Urgent fix to something already released — e.g., a broken CI pipeline or credential rotation on `main`. |
| `release/*`   | `develop`    | `main` **and** `develop` | Temporary | Stabilization before a release. Only bugfixes and release-prep changes (version bump, changelog) allowed — no new features. |
| `chore/*`     | `develop`    | `develop`           | Temporary  | Non-functional changes: dependency bumps, CI/tooling tweaks, lint config, `.gitignore` updates. |
| `docs/*`      | `develop`    | `develop`           | Temporary  | Documentation-only changes (README, CONTRIBUTING, BUILD_LOG backfills). |

### Naming convention

```
<type>/<ticket-id>-<short-kebab-case-description>
```

- All lowercase, words separated by hyphens
- No spaces, no underscores, no camelCase
- Ticket ID included when work is tied to a tracked item; omit for `chore/*`/`docs/*`
  with no ticket
- Keep total length under ~50 characters

**Examples:**
```
feature/PCF-104-cart-checkout-flow
framework/PCF-020-custom-world-and-hooks
bugfix/PCF-133-login-step-flaky-wait
hotfix/PCF-201-ci-secret-rotation
release/1.2.0
chore/upgrade-playwright-to-1-48
docs/update-readme-run-commands
```

### Merge direction diagram

```
main ────────────●─────────────────────●───────────────►  (tagged releases)
                   \                   / \
                    \                 /   \
release/1.0.0 ───────●───────────────●     \
                     /                       \
develop ────●───────●───●───●───●─────●───────●──────────►
            /       /   /   /   /     /
   feature/*   framework/*  bugfix/*  ...

hotfix/* branches off main directly, merges back into BOTH main and develop.
```

### Rules

1. **No direct commits to `main` or `develop`.** All changes go through a pull
   request from a supporting branch.
2. **`feature/*` and `framework/*` branches must be up to date with `develop`**
   before opening a PR (rebase or merge `develop` in).
3. **`release/*` branches only accept bugfixes**, version bump commits, and
   `BUILD_LOG.md`/`README.md` updates — no new test coverage.
4. **`hotfix/*` is the only branch type allowed to branch off `main` directly**
   (besides `release/*`), and must always be merged back into both `main` and
   `develop` to avoid regressions being reintroduced.
5. Delete the branch after merge (GitHub's "delete branch" on merge is enabled
   by default in this repo).

---

## Commit Message Convention

This repo follows **Conventional Commits**:

```
<type>(<optional-scope>): <short description>

[optional body]

[optional footer(s)]
```

**Types:** `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `ci`, `perf`, `style`

**Examples:**
```
feat(cart): add checkout flow steps and page object
fix(login): correct locked-out-user error message assertion
framework(hooks): add trace capture on scenario failure
chore(deps): bump playwright to 1.48.0
docs(readme): document tag strategy
ci(github-actions): add smoke test workflow on PR
```

Every commit that changes framework structure, adds a phase of the build, or
lands a meaningful feature should have a corresponding entry appended to
[`BUILD_LOG.md`](./BUILD_LOG.md).

---

## Pull Request Rules

- PRs target `develop` (or `main`/`develop` for `hotfix/*` and `release/*`, as
  two separate PRs).
- Use the PR template (`.github/PULL_REQUEST_TEMPLATE.md`) — it requires linking
  a ticket (if applicable), describing the change, and confirming tests pass
  locally.
- At least **1 approval** required (see branch protection rules in README).
- All CI status checks must pass.
- Squash-merge is preferred for `feature/*`/`bugfix/*`/`chore/*`/`docs/*` into
  `develop` to keep history readable; `release/*` and `hotfix/*` merges into
  `main` use a merge commit (preserves the release point for tagging).

---

## Naming Conventions (Code)

See the **Naming Conventions** section of `README.md` (added in Phase 1) for
file/class/method naming rules once the TypeScript scaffolding lands.

---

## Code Review Checklist

Reviewers should confirm:
- [ ] No hardcoded credentials, URLs, or environment-specific values
- [ ] New step definitions are reusable (no scenario-specific logic hardcoded into steps)
- [ ] Page objects extend `BasePage` and don't duplicate existing methods
- [ ] Appropriate tags applied (`@smoke`/`@regression`/`@in-sprint`/etc.)
- [ ] `BUILD_LOG.md` updated if this PR represents a meaningful framework milestone
- [ ] No `.only`/`.skip` left in committed code
- [ ] Logs added at appropriate levels for new page interactions
