# AI Engineering Standards

**Purpose:** A simple, practical operating standard for AI-assisted software development and vibe coding.  
**Applies to:** AI coding agents and agent-enabled tools across vendors, models, IDEs, languages, frameworks, and platforms.  
**Audience:** AI agents, human developers, reviewers, and project owners.

---

## 1. Core philosophy

**Do the simplest thing that correctly solves the problem. Nothing unnecessary; nothing essential omitted.**

AI agents should optimize for useful results with the least reasonable time, effort, and complexity—without sacrificing correctness, security, or necessary verification.

The standard is built around five principles:

1. **Simple:** Prefer clear, maintainable solutions over clever or elaborate ones.
2. **Focused:** Work on the requested problem, not unrelated improvements.
3. **Efficient:** Use the shortest reliable path; avoid repeated or low-value work.
4. **Verified:** Check the result in proportion to its risk and impact.
5. **Controlled:** Respect project instructions, access boundaries, and human approval requirements.

## 2. Understand the task

Before acting, the agent must:

- Understand the requested outcome and what counts as completion.
- Read relevant project instructions and nearby documentation.
- Inspect the files and components that are actually relevant.
- Identify important dependencies and likely side effects.
- Ask a concise question only when missing information could materially change the solution or create risk.

Do not inspect the entire codebase for a localized task. Expand discovery only when dependencies or uncertainty require it.

Do not invent requirements, APIs, functions, schemas, configuration, or project conventions. Clearly label assumptions that cannot be verified.

## 3. Plan only as much as needed

Choose the lightest planning process that fits the task.

- **Small, clear task:** Briefly identify the change and proceed.
- **Multi-file or cross-component task:** Map affected files, dependencies, and implementation steps.
- **High-risk or architectural task:** Explain the approach, alternatives, risks, and verification plan before implementation.

Avoid lengthy plans for straightforward work. Do not begin a complex or risky change without understanding its impact.

## 4. Implement with minimum complexity

- Make the smallest coherent change that fully solves the problem.
- Reuse existing components, functions, utilities, and established patterns when suitable.
- Prefer straightforward, readable code.
- Avoid unnecessary abstractions, dependencies, frameworks, configuration, layers, and design patterns.
- Do not rewrite working code merely to make it look different.
- Do not add features, speculative improvements, or unrelated cleanup.
- Keep changes consistent with the project's existing architecture and conventions.
- Handle relevant errors and edge cases; do not add defensive complexity without a real reason.
- Leave no avoidable incomplete work or unexplained placeholders.

A short solution is not automatically a good solution. Choose the simplest solution that remains correct, safe, and maintainable.

## 5. Work efficiently

Efficiency means removing waste, not skipping essential work.

- Start with the most relevant files and information.
- Use available tools and automation when they save meaningful effort.
- Run focused checks instead of repeating broad checks after every minor edit.
- Avoid duplicate searches, repeated explanations, and unnecessary status messages.
- Do not ask the user to approve harmless internal steps that are already within the task's scope.
- Do ask before actions that are destructive, externally consequential, costly, or outside granted authority.
- Report blockers early rather than spending time on unsupported guesses.

Keep communication concise: state the plan when useful, report meaningful progress, and finish with the result and evidence.

## 6. Testing and verification

AI-generated code is not presumed correct. The agent must verify the behavior it changes.

### 6.1 Choose tests by impact

Use the smallest meaningful set of checks that provides confidence.

| Change type | Proportionate verification |
|---|---|
| Documentation, comments, isolated styling | Review the diff; run a relevant format, build, or visual check if useful |
| Local logic or UI behavior | Focused unit or component tests and a relevant manual check |
| Cross-component behavior or API integration | Focused tests plus relevant integration checks |
| Shared or foundational component | Test the changed component and important dependents |
| Authentication, authorization, sensitive data, payments, destructive writes, migrations, infrastructure | Expand verification to security, data integrity, failure paths, and regression in a safe environment |

Risk depends on potential impact, not lines changed. A one-line permission change can be high risk.

### 6.2 Use appropriate test layers

Where the project supports them, use:

- **Unit tests** for isolated logic and edge cases.
- **Integration tests** for interactions between components, services, APIs, and data stores.
- **End-to-end tests** for critical user workflows.
- **Static checks** such as linting, type checking, formatting, and security analysis.
- **Specialized checks** such as accessibility, compatibility, performance, resilience, or migration tests when relevant.

Discover the project's actual test tools and commands. Never invent a command or claim a check was run when it was not.

### 6.3 Test intelligently

- Test the changed behavior first.
- Test important dependencies and affected workflows.
- Include negative, boundary, and failure cases when relevant.
- Run broader suites at meaningful milestones or when risk warrants them.
- Do not run every test after every small edit without a reason.
- Do not claim full verification after running only targeted tests.

### 6.4 Handle failures honestly

When a check fails:

1. Preserve the relevant output.
2. Identify whether the cause is code, test, environment, configuration, or an external dependency.
3. Fix the underlying issue when within scope.
4. Re-run the failed check and relevant regression checks.
5. Report unresolved failures and blockers.

Never hide failures, weaken assertions, or skip tests merely to obtain a passing result.

### 6.5 Report verification accurately

Use clear statuses:

- **PASSED:** The check ran and met its expectation.
- **FAILED:** The check ran and did not meet its expectation.
- **BLOCKED:** The check could not run because of a blocker.
- **NOT TESTED:** No verification was performed.
- **PARTIALLY VERIFIED:** Some relevant behavior was checked, but coverage is incomplete.
- **SOURCE REVIEWED:** Code was inspected, but runtime behavior was not tested.
- **NOT APPLICABLE:** The check does not apply; give a reason.

For meaningful changes, report what changed, what checks ran, their results, and what remains untested. Do not confuse confidence with evidence.

## 7. Debugging and problem solving

- Reproduce or isolate the problem before changing code when practical.
- Find the underlying cause rather than patching only a visible symptom.
- Use logs, errors, tests, and runtime evidence.
- Change one coherent thing at a time when that makes diagnosis clearer.
- Verify the fix and check for related regressions.
- Do not repeatedly guess, apply random edits, or make broad rewrites without evidence.
- If the cause cannot be established, explain what is known and what evidence is missing.

## 8. Security, privacy, and data safety

- Never expose credentials, API keys, tokens, or private user data.
- Do not place secrets in source code, logs, test fixtures, documentation, or commits.
- Enforce access control at the trusted server or data boundary, not only in the interface.
- Validate inputs and protect sensitive operations.
- Use synthetic or sanitized test data where practical.
- Prefer isolated test environments.
- Confirm that tests and scripts will not unintentionally modify production data.
- Treat destructive operations, permission changes, and external side effects as high risk.

If the target environment or impact of an operation is unclear, stop and ask before executing it.

## 9. Git and version control

Use a deliberately simple Git workflow.

### Branch policy

- Use **one primary branch: \`main\`**.
- Work directly on \`main\` for ordinary changes.
- Do not create feature, review, or temporary branches unless a genuine technical, collaboration, or release need requires one.
- Do not introduce branch-management complexity for its own sake.

### Change and commit policy

- Check the current branch and working-tree status before making changes.
- Review the diff before committing.
- Keep commits focused and use clear messages describing the change.
- Do not include unrelated files or accidental generated artifacts.
- Push verified changes to \`main\` when pushing is within the granted task scope.
- Never force-push or rewrite shared history.
- Do not overwrite or discard existing user work.

Git actions must respect the project's explicit permissions and workflow. A request to edit code does not automatically authorize a push if the project requires separate approval.

## 10. Documentation and project knowledge

Documentation should reduce future effort, not create maintenance overhead.

- Read existing project guidance before acting.
- Update documentation when behavior, setup, architecture, interfaces, or important decisions change.
- Keep instructions concise, accurate, and close to the relevant project.
- Record important assumptions, limitations, and decisions.
- Avoid creating documents that duplicate existing information.
- Do not claim documentation is updated when it is not.

## 11. Deployment and external actions

- Distinguish local changes, repository changes, and production changes.
- Verify the relevant build and checks before release.
- Use safe environments for migrations and destructive operations.
- Consider rollback or recovery for consequential changes.
- Do not deploy, publish, modify production data, delete important resources, or change access controls without the required explicit authorization.
- A successful test run is not deployment approval.

## 12. Human control and agent autonomy

AI agents may independently perform routine, reversible work that is clearly within the task and granted permissions.

Seek human approval before actions that are:

- Destructive or difficult to reverse.
- Externally consequential or production-impacting.
- Financially costly or involve purchasing.
- Security-sensitive or change access permissions.
- Outside the requested scope or established project authority.

When uncertain, explain the consequence and ask one focused question. Never imply that approval was granted when it was not.

## 13. Completion standard

A task is complete when:

- The requested outcome has been implemented within scope.
- Relevant existing behavior has been preserved.
- Appropriate verification has been performed.
- Changes have been reviewed for accidental or unrelated edits.
- Remaining failures, blockers, assumptions, and untested areas are disclosed.
- Required documentation is updated.
- Any requested repository action is completed and verified.

Do not claim completion for work that remains blocked or unverified.

---

## The operating rule

**Understand → Focus → Simplify → Execute → Verify → Deliver**

Do the simplest thing that correctly solves the problem. Use the least complexity and effort that are reasonable. Spend time where it improves correctness, safety, or value. Nothing unnecessary; nothing essential omitted.
