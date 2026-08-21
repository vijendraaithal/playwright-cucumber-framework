# Playwright + Cucumber BDD Test Automation Framework

Enterprise-grade UI test automation framework for **saucedemo.com**, built with
[Playwright](https://playwright.dev/) and [Cucumber.js](https://github.com/cucumber/cucumber-js)
using the Behavior-Driven Development (BDD) approach, in TypeScript.

> 📌 Status: **Under active build.** See [BUILD_LOG.md](./BUILD_LOG.md) for the full,
> chronological history of how this framework was constructed, phase by phase.

---

## Table of Contents

- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Environment Configuration](#environment-configuration)
- [Running Tests](#running-tests)
- [Tag Strategy](#tag-strategy)
- [Reporting](#reporting)
- [Branching Strategy (GitFlow)](#branching-strategy-gitflow)
- [Contributing](#contributing)
- [CI/CD](#cicd)

---

## Tech Stack

| Layer            | Tool/Library                          |
|-------------------|----------------------------------------|
| Browser Automation | Playwright                            |
| BDD Runner         | Cucumber.js                           |
| Language           | TypeScript                            |
| Assertions         | Playwright's built-in `expect`        |
| Reporting          | Allure Reports                        |
| Logging            | Winston                               |
| CI (current)       | GitHub Actions                        |
| CI (planned)       | Azure Pipelines + Azure Static Web Apps |
| Secrets (local)    | `.env` files (gitignored)             |
| Secrets (CI)       | GitHub Encrypted Secrets → Azure Key Vault (planned) |

## Project Structure

```
playwright-cucumber-framework/
├── features/          # Gherkin .feature files, organized by module
├── config/             # cucumber.js, playwright.config.ts, env files
├── src/
│   ├── steps/          # Step definitions (glue code), mirrors features/
│   ├── pages/           # Page Object Model classes (BasePage + concrete pages)
│   ├── support/
│   │   ├── world/        # Custom Cucumber World + fixtures
│   │   └── hooks/        # Before/After/BeforeAll/AfterAll hooks
│   ├── api/              # API helpers (e.g. login for storageState reuse)
│   └── utils/             # Logger, env config loader, data helpers
├── data/               # Static test data, data builders/factories
├── reports/             # Allure results/report (gitignored)
├── test-results/        # Playwright traces, videos, screenshots (gitignored)
└── .github/             # Workflows, PR/issue templates, CODEOWNERS
```

> Full naming conventions are documented in [CONTRIBUTING.md](./CONTRIBUTING.md).

## Prerequisites

- Node.js (version pinned in [`.nvmrc`](./.nvmrc)) — use `nvm use`
- npm (bundled with Node)
- Git

## Getting Started

```bash
git clone https://github.com/<your-username>/playwright-cucumber-framework.git
cd playwright-cucumber-framework
nvm use
npm install
npx playwright install --with-deps
cp .env.example config/env/.env.dev
# fill in config/env/.env.dev with real (non-production) test credentials
```

## Environment Configuration

This framework never commits real credentials. See [`.env.example`](./.env.example)
for the full list of expected variables and where each layer (local / GitHub Actions /
Azure Pipelines) sources them from.

## Running Tests

> ⚠️ Detailed run commands will be documented here once the test execution layer
> (Phase 4 onward) is built. Placeholder for now:

```bash
npm run test:smoke
npm run test:regression
npm run report:generate
npm run report:open
```

## Tag Strategy

| Tag             | Purpose                                      |
|------------------|-----------------------------------------------|
| `@smoke`         | Critical-path, fastest subset                |
| `@sanity`        | Slightly broader post-deploy checks           |
| `@regression`    | Full functional coverage                      |
| `@in-sprint`     | Tests for stories currently in active sprint  |
| `@n-1`           | Tests for previous sprint's stabilized stories |
| `@wip`           | Work in progress — excluded from CI runs      |
| `@flaky-quarantine` | Known-flaky, isolated from main run gates  |

Full detail to be added in Phase 3.

## Reporting

- **Local**: Allure reports generated from `reports/allure-results`.
- **CI (GitHub Actions)**: Allure report published as a workflow artifact.
- **CI (Azure Pipelines, planned)**: Allure report published to an Azure Static Web App,
  accessible to the team via a shared URL.

## Branching Strategy (GitFlow)

See [CONTRIBUTING.md](./CONTRIBUTING.md#branching-strategy) for full branch naming rules,
merge direction, and protection rules on `main` and `develop`.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## CI/CD

See [`.github/workflows`](./.github/workflows) for current GitHub Actions pipelines.
Azure DevOps / Azure Pipelines integration is planned as a later phase — see BUILD_LOG.md.
