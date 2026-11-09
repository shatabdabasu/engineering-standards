# Reusable Testing Environment Standard

## 1. Purpose

Give every maintained project a repeatable, documented way to prepare its test environment, run checks, and understand test results. The goal is to stop rediscovering setup requirements on every task.

This standard defines the process, not one universal toolchain. Each project selects tools that fit its actual language, framework, platform, and risk.

## 2. Required outcome

Before repeated development work relies on tests, an agent must be able to find a project testing profile that answers:

- Which language/runtime and version are required?
- Which package manager, dependency manifest, and lockfile are authoritative?
- Which test, lint, type-check, build, and static-analysis tools are installed?
- What are the exact setup and execution commands?
- Which environment variables, credentials, services, APIs, emulators, or accounts are required?
- How are test data and resources isolated, seeded, reset, and cleaned up?
- Which tests are local, mocked, integration, browser/device, or live-environment tests?
- Which operations are prohibited without explicit authorization?
- How can the agent prove that the test environment itself is operational?

Unknown details must be marked **UNKNOWN** and investigated. Agents must not invent commands, tool versions, APIs, or project requirements.

## 3. Establish the environment once

### Step 1 — Inspect before installing

Inspect the repository and current machine for:

- Runtime/version files and existing toolchain declarations.
- Package manifests and lockfiles.
- Test runner and test configuration.
- Lint, formatting, type-check, build, and static-analysis configuration.
- Existing scripts, Makefiles, task runners, and CI workflows.
- Environment examples, container/service configuration, and setup documentation.
- Fixtures, seed data, mocks, emulators, and existing test utilities.

Check existing changes and project conventions before modifying setup. Do not replace working tools merely because another tool is familiar.

### Step 2 — Choose the smallest suitable toolset

Select tools according to the actual stack and test requirements:

- Unit test runner for isolated logic.
- Integration test support for component and service boundaries.
- Browser automation when real browser workflows need verification.
- Linters, type checkers, and static analyzers where they provide value.
- Official platform emulators or test environments when platform APIs cannot be represented faithfully by simple mocks.

Prefer tools already used by the project. Add a dependency only when its purpose and benefit are clear. Avoid duplicate test runners and unnecessary infrastructure.

### Step 3 — Make setup reproducible

Where the ecosystem supports it:

- Declare the required runtime version or supported range.
- Maintain the dependency manifest and lockfile.
- Prefer project-local dependencies over undeclared global installations.
- Provide one documented setup path.
- Provide exact commands for fast checks and the relevant full test suite.
- Keep test configuration separate from production configuration.
- Keep secrets out of source control; document required variable names and safe ways to supply them.
- Pin versions when reproducibility or compatibility requires it.

Do not upgrade unrelated dependencies, alter global machine configuration, or add heavyweight containers/services without a demonstrated need and appropriate authorization.

### Step 4 — Isolate services and test data

For tests that write data or call external services, use disposable or isolated resources wherever feasible.

Document:

- Required local services, emulators, test accounts, and credentials.
- How fixtures or seed data are created.
- How data and resources are reset and cleaned up.
- Which calls can incur cost or affect external users.
- Which tests must never target production.

Use synthetic or anonymized fixtures. Do not copy sensitive production data into an unsafe test environment. Never run destructive tests against production.

### Step 5 — Prove the environment works

Create a small, deterministic smoke test that can be run with a documented command. It should prove that the test runner starts and executes a known-good test successfully.

The smoke test validates the test environment only. It does not prove that application features work.

The environment is **READY FOR TESTING** only when:
1. Setup completes using the documented instructions.
2. The smoke test passes.
3. Required test commands are discoverable and runnable.
4. Any unavailable integration or live dependencies are clearly recorded.

## 4. Validate the test infrastructure

A test harness, mock, fixture, emulator, and test script are software too. They must be reviewed.

- Inventory the real application APIs and behaviors used by each test.
- Ensure mocks implement the required contract, including method signatures, return shapes, state changes, and relevant failure behavior.
- Keep mocks faithful to the platform; never add behavior merely to make an assertion pass.
- Run a known-good smoke test before feature assertions.
- Separate setup errors, harness errors, assertion failures, and application failures.
- If execution stops before assertions run, report the test as **BLOCKED / INFRASTRUCTURE FAILURE**, not as an application defect.
- Do not repair mocks one missing method at a time without reviewing the required API surface and overall test strategy.
- If maintaining the harness costs more than a simpler credible test approach, reassess it. Consider a focused test, official emulator, disposable integration environment, or authorized test instance.
- Never change production application code solely to satisfy an inaccurate mock.

## 5. Use a layered testing strategy

Choose only the layers needed for the change and its risk.

| Layer | Purpose | What it does not prove |
|---|---|---|
| Static checks | Syntax, lint, types, formatting, static analysis | Runtime behavior |
| Unit / mocked | Isolated business logic and edge cases | Real platform/service behavior |
| Integration | Interactions across components or real service interfaces | Full deployed user experience |
| Browser / device | Actual UI workflows on target browsers/devices | All production conditions |
| Live / production | Deployed system with real services | Must be authorized; never implied by local tests |

Use the fastest useful checks first. Run broader and more expensive checks when the change's risk or acceptance criteria require them.

## 6. Project-specific testing profile

Every maintained project should have a concise testing profile, preferably at a predictable path such as `docs/TESTING.md` or `TESTING.md`.

The profile should record:

- Runtime and version.
- Dependency manager and install/restore command.
- Test tools and their versions where material.
- Exact setup, smoke-test, targeted-test, full-test, lint, type-check, and build commands.
- Required configuration and environment variables, without secret values.
- Required services, emulators, test accounts, and access boundaries.
- Fixture, seed, reset, and cleanup procedures.
- Mock limitations and real-platform differences.
- CI workflow and checks, if present.
- Tests that remain unavailable or require human authorization.

Keep the profile current when setup changes. Do not copy the entire universal standard into every project.

## 7. Task-level test plan

For each meaningful change, add a proportionate test plan to the task record or pull request:

1. Behavior changed and acceptance criteria.
2. Relevant tests and expected outcomes.
3. Dependencies and existing workflows requiring regression checks.
4. Required environment and test data.
5. Tests executed and evidence.
6. Remaining untested paths and why they remain untested.

Small isolated changes may use a short plan. Data integrity, authorization, security, migration, and production-critical changes require deeper coverage.

## 8. Failure classification

Do not report every failed test as an application defect. Classify the result accurately:

- **APPLICATION FAILURE:** The test executed the relevant application behavior and an assertion or expected outcome failed.
- **TEST INFRASTRUCTURE FAILURE:** The runner, setup, mock, fixture, emulator, or harness failed before a valid application assertion could be completed.
- **ENVIRONMENT / INTEGRATION BLOCKER:** A required service, credential, permission, network, platform, or test account was unavailable.
- **TEST SPECIFICATION FAILURE:** The test expectation is incorrect, ambiguous, stale, or inconsistent with approved requirements.
- **PASSED:** The test completed and evidence supports the expected result.
- **UNTESTED:** The scenario was not executed.

A single task may have multiple classifications. Explain the evidence and do not hide infrastructure failures.

## 9. CI and repeatability

Where a project uses CI, run the same core checks in CI that developers are expected to run locally, subject to platform limitations. Avoid separate, drifting command sets.

CI should use declared runtime versions, dependency lockfiles, safe test configuration, and non-production credentials. Keep secrets in the CI secret store, not in repository files or logs.

Do not add CI solely for appearance. Add it when automated repeatability, collaboration, or release confidence justifies the maintenance cost.

## 10. Maintenance

When changing dependencies, runtime versions, test tools, mocks, fixtures, or CI:

- Update the project testing profile.
- Verify setup from a clean or representative environment when practical.
- Run the smoke test and affected test suites.
- Review compatibility and security implications.
- Record material limitations and migration steps.

At the beginning of each new task, inspect and reuse the established setup. Reassess only what has changed or is demonstrably broken.

## 11. Completion checklist

A project testing environment is established when:

- [ ] Runtime and dependency requirements are documented.
- [ ] Setup and test commands are documented and reproducible.
- [ ] Test tools are selected for the actual project stack.
- [ ] Test configuration is separated from production configuration.
- [ ] Test data and external services are isolated safely.
- [ ] A smoke test passes.
- [ ] Mock limitations and live-test requirements are documented.
- [ ] CI checks are aligned with local checks, if CI is used.
- [ ] A project testing profile exists and has an owner through normal project maintenance.

A task's verification is complete only to the extent supported by executed tests and evidence. Environment readiness must never be confused with application correctness.
