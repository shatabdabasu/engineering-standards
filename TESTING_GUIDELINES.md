# Testing Guidelines

**Scope:** Universal engineering standard  
**Applies to:** Software projects across languages, frameworks, platforms, and deployment environments

---

## 1. Purpose

These guidelines establish a practical, risk-based approach to software testing and verification. They apply to web, mobile, desktop, backend, API, database, automation, cloud, AI, data-processing, and enterprise systems.

The objective is not to run every test after every edit. It is to detect defects early, verify affected behavior efficiently, protect existing functionality, and gather enough evidence to make reliable engineering decisions.

Project-specific test plans may extend these guidelines but should not weaken their core principles without an explicit, documented reason.

## 2. Core principles

1. **Understand before testing.** Identify the behavior, requirements, dependencies, and possible side effects before choosing tests.
2. **Test according to risk.** Give more attention to changes that can affect security, data integrity, money, privacy, availability, or critical workflows.
3. **Prefer fast feedback.** Run small, targeted checks during implementation and broader checks at meaningful milestones.
4. **Automate repeatable checks.** Use automated tests for behavior that must be verified repeatedly.
5. **Verify observable behavior.** A test should establish whether expected behavior occurred, not merely whether code executed.
6. **Preserve existing behavior.** Include regression checks for important functionality affected by a change.
7. **Keep evidence.** Record what was tested, the result, the environment, and any limitations.
8. **Never confuse absence of failure with proof of correctness.** Untested behavior remains untested.

## 3. Testing workflow

Use a verification cycle appropriate to the size and risk of the task:

### Before implementation
- Understand the requested behavior and acceptance criteria.
- Inspect relevant code, dependencies, interfaces, and data flows.
- Identify affected components and likely regression areas.
- Identify available tests and the correct way to run them.
- Note security, privacy, compatibility, and data-integrity risks.

### During implementation
- Run focused checks after meaningful changes.
- Verify the smallest affected unit or behavior first.
- Use syntax, type, lint, build, or static-analysis checks where applicable.
- Add or update automated tests for new or changed behavior.
- Avoid repeatedly running unrelated, expensive test suites after trivial edits.

### After implementation
- Run tests covering the changed behavior.
- Run relevant integration tests where components interact.
- Run targeted regression tests for affected existing behavior.
- Review logs and test output; do not rely only on exit codes.
- Check error paths and boundary conditions proportionate to risk.
- Record failures, blockers, and untested areas honestly.

### Before release or deployment
- Run the broader test suite required by the project's release policy.
- Verify build/package artifacts and configuration.
- Check migration, rollback, backup, and recovery requirements when relevant.
- Confirm that required security and compatibility checks have passed.
- Obtain required human approval before production-impacting actions.

## 4. Risk-based testing

Classify each change by its potential impact. When uncertain, choose the higher level until the risk is understood.

| Risk level | Typical examples | Expected verification |
|---|---|---|
| Low | Documentation, comments, isolated styling, non-functional text | Review the change; run relevant formatting, build, or visual check if applicable |
| Medium | Form behavior, UI interaction, business rules, calculations, API behavior | Focused automated tests, relevant integration checks, and targeted regression tests |
| High | Authentication, authorization, sensitive data, payments, database writes/deletes, migrations, infrastructure, production configuration | Thorough unit and integration coverage, security checks, data-integrity checks, failure-path tests, targeted regression, and controlled environment verification |

Risk is determined by impact and likelihood, not by the number of lines changed. A one-line permission check can be higher risk than a large cosmetic change.

## 5. Testing layers

Use the testing layers that fit the system's architecture.

### Unit tests
Verify a small unit of behavior in isolation. They should be fast, deterministic, and focused on inputs, outputs, and relevant edge cases.

### Integration tests
Verify interactions between components, services, APIs, databases, queues, external adapters, or other system boundaries. Use controlled dependencies and test data.

### End-to-end tests
Verify critical user or system workflows across the integrated application. Keep these focused on important journeys because they are typically slower and more operationally fragile.

### Other useful checks
Depending on the project, include:
- Static analysis, linting, formatting, and type checking.
- Build and packaging verification.
- Accessibility and usability checks.
- Compatibility tests across supported platforms, browsers, or runtimes.
- Performance, load, stress, and resilience tests.
- Security scanning and focused security testing.
- Data validation and migration checks.

Do not add a test category merely for appearance. Choose checks that address an actual project risk.

## 6. Test selection and efficiency

Use the smallest meaningful test set that provides confidence for the current change.

- For a localized change, start with the directly affected tests.
- For a change crossing component boundaries, include relevant integration tests.
- For a change to a shared utility or foundational module, identify and test its major dependents.
- For a high-risk change, expand coverage to security, data integrity, failure handling, and regression.
- Run the full suite at appropriate milestones, such as before release or when the change has broad impact.

A test suite should be selected based on impact and dependency, not habit. Do not claim full verification when only targeted tests were run.

## 7. Regression testing

Regression testing checks that a change has not broken behavior that previously worked.

- Identify existing behavior that depends on the modified component.
- Add regression tests for defects once fixed, where practical.
- Prioritize critical user journeys and shared interfaces.
- Re-run relevant tests when shared code, schemas, contracts, or configuration changes.
- Document known gaps where regression coverage is unavailable.

## 8. Test data and environments

- Prefer isolated, reproducible test environments.
- Use synthetic or sanitized data where practical.
- Never expose real credentials, secrets, or sensitive personal data in test fixtures or logs.
- Make test setup and cleanup explicit.
- Avoid tests that depend on uncontrolled external state.
- Ensure tests do not unintentionally modify production data or services.
- Use mocks or fakes for isolation where appropriate, but retain integration tests for important real interactions.
- Document environment assumptions, versions, and required services.

## 9. Negative, boundary, and failure-path testing

Do not test only the expected successful path. Where relevant, verify:

- Empty, missing, malformed, and invalid inputs.
- Minimum, maximum, and out-of-range values.
- Unauthorized and unauthenticated access.
- Duplicate submissions and repeated operations.
- Timeouts, network failures, unavailable services, and retries.
- Partial failures and interrupted workflows.
- Concurrent requests and conflicting updates.
- Invalid state transitions and recovery behavior.

Select cases according to the behavior and risk; not every item applies to every feature.

## 10. Automation standards

Automate checks that are repeatable, valuable, and stable enough to maintain.

- Keep automated tests deterministic wherever possible.
- Avoid arbitrary sleeps when synchronization or condition-based waits are available.
- Make failures diagnosable through useful assertions and output.
- Keep test names and structure understandable.
- Separate test setup, execution, and cleanup.
- Avoid tests that pass without actually exercising the intended behavior.
- Keep test execution practical for the development workflow.
- Review flaky tests; do not normalize repeated intermittent failures.

Automation supports engineering judgment; it does not replace code review, exploratory testing, or production safeguards.

## 11. Security and data-integrity verification

For changes involving identity, permissions, sensitive information, persistence, or external access:

- Verify authorization at the trusted enforcement boundary, not only in the user interface.
- Test both permitted and denied actions.
- Check that users and components cannot access data beyond their authorized scope.
- Validate inputs and enforce relevant constraints at the appropriate layer.
- Verify that failed operations do not leave unintended partial state.
- Check that logs and error messages do not disclose secrets or sensitive data.
- Test migrations and destructive operations in a safe, representative environment.
- Confirm backup and recovery procedures where the change can affect critical data.

Security-sensitive changes require evidence beyond a successful happy-path test.

## 12. Failure handling

When a test fails:

1. Preserve the failure output and relevant context.
2. Determine whether the failure is caused by the change, the test, the environment, or an external dependency.
3. Reproduce the failure where practical.
4. Do not silently skip, weaken, or delete a failing test simply to obtain a green run.
5. Fix the underlying issue or document a justified exception.
6. Re-run the failed test and the relevant regression set after the correction.

A test that could not run because of an environment or dependency issue is blocked, not passed.

## 13. Verification result vocabulary

Use explicit, consistent result labels in reports:

| Status | Meaning |
|---|---|
| **PASSED** | The check ran and met its stated expectation |
| **FAILED** | The check ran and did not meet its expectation |
| **BLOCKED** | The check could not run because of an identified blocker |
| **NOT TESTED** | No verification was performed |
| **PARTIALLY VERIFIED** | Some relevant behavior was checked, but coverage is incomplete |
| **SOURCE REVIEWED** | Code or configuration was inspected, but runtime behavior was not tested |
| **NOT APPLICABLE** | The check does not apply; provide a brief reason |

Do not report a build, test, security property, or deployment as successful unless supporting evidence exists.

## 14. Verification reporting

For meaningful tasks, record:

- Task or change identifier and date.
- Summary of the change and affected components.
- Risk level and reason.
- Tests and commands performed.
- Environment and relevant versions.
- Result for each check.
- Failures, warnings, and blockers.
- Untested areas and known limitations.
- Overall verification assessment.
- Reviewer or approval information when required.

Keep reports factual. Separate observed results from assumptions and recommendations.

## 15. Release and production safeguards

- Test in a non-production environment before production when feasible.
- Verify configuration and secrets handling for the target environment.
- Confirm that database or data migrations have a tested recovery strategy.
- Use staged or gradual rollout methods when appropriate.
- Monitor relevant logs, metrics, and critical workflows after release.
- Define rollback or mitigation steps for material risks.
- Do not deploy, modify production data, or perform destructive actions without the required authorization.

A successful test suite is not, by itself, authorization to deploy.

## 16. Continuous improvement

Testing practices should evolve with the system.

- Track recurring defects and add preventive tests where practical.
- Review escaped defects to identify missing checks.
- Remove obsolete or misleading tests.
- Improve slow or flaky tests when they impede feedback.
- Measure coverage as one signal, not as proof of quality.
- Prefer meaningful assertions and risk coverage over arbitrary coverage targets.

## 17. Final standard

The expected engineering approach is:

**Understand the change. Assess its risk. Test the affected behavior first. Expand verification according to dependencies and impact. Automate repeatable checks. Preserve evidence. Be explicit about what remains untested.**

The goal is not maximum testing activity. The goal is reliable software, achieved with efficient and proportionate verification.
