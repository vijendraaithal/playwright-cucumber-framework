## Summary

<!-- What does this PR do? Why is it needed? -->

## Related Ticket

<!-- e.g. PCF-104, or "N/A" -->

## Type of Change

- [ ] `feature` — new test coverage / new capability
- [ ] `framework` — core framework infrastructure change
- [ ] `bugfix` — fixes a bug in test code or framework
- [ ] `hotfix` — urgent fix to already-released code
- [ ] `chore` — dependency bump, tooling, non-functional
- [ ] `docs` — documentation only

## Checklist

- [ ] Branch is up to date with `develop`
- [ ] No hardcoded credentials, URLs, or environment-specific values
- [ ] New/changed step definitions are reusable (no scenario-specific logic hardcoded)
- [ ] Page objects extend `BasePage`, no duplicated methods
- [ ] Appropriate tags applied (`@smoke` / `@regression` / `@in-sprint` / `@n-1` / etc.)
- [ ] Ran tests locally and they pass
- [ ] No `.only` / `.skip` left in committed code
- [ ] `BUILD_LOG.md` updated (if this PR is a meaningful framework milestone)
- [ ] `README.md` updated (if this PR changes setup/run/tag instructions)

## Screenshots / Evidence (optional)

<!-- Allure report screenshot, trace snippet, etc. -->

## Notes for Reviewers

<!-- Anything reviewers should pay special attention to -->