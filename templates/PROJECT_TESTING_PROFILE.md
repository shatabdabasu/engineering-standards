# Project Testing Profile

> Keep this file specific to this repository. Link to the organization's Reusable Testing Environment Standard instead of copying it.

## 1. Project and ownership

- Project:
- Repository:
- Last verified:
- Maintainer:
- Testing profile status: UNKNOWN / SETUP REQUIRED / READY FOR TESTING

## 2. Runtime and dependencies

- Language/framework:
- Required runtime version:
- Package/dependency manager:
- Manifest and lockfile:
- Install/restore command:
- Dependency upgrade policy:

## 3. Test tools

| Purpose | Tool | Version / source | Command |
|---|---|---|---|
| Smoke test | | | |
| Unit tests | | | |
| Integration tests | | | |
| Lint / static analysis | | | |
| Type checks | | | |
| Build | | | |
| Browser / device tests | | | |

Do not list tools that the project does not use.

## 4. Environment setup

- Prerequisites:
- Setup command(s):
- Required environment variables (names only; never put secret values here):
- Local services / emulators:
- Test accounts / access required:
- Configuration files:

## 5. Test data and safety

- Fixture / seed data location:
- Data setup procedure:
- Reset / cleanup procedure:
- External services called:
- Potential costs or external side effects:
- Production restrictions:

## 6. Test commands

| Purpose | Exact command / procedure | Expected result |
|---|---|---|
| Environment smoke test | | |
| Fast targeted tests | | |
| Full relevant test suite | | |
| Static / syntax checks | | |
| Browser / device tests | | |
| Integration / live tests | | |

## 7. Test architecture and limitations

- Mocked APIs and their source:
- Important mock limitations:
- Real platform behaviors not represented by mocks:
- Integration tests requiring external services:
- Browser/device coverage:
- Known flaky or unavailable tests:

## 8. CI

- CI provider/workflow:
- Trigger:
- Checks executed:
- Secret/configuration requirements:
- Differences from local execution:

## 9. Verification evidence

| Date | Environment | Command / scenario | Result (PASSED / FAILED / UNTESTED / BLOCKED) | Evidence / limitation |
|---|---|---|---|---|
| | | | | |

## 10. Maintenance notes

- When setup last worked from a clean or representative environment:
- Known setup issues:
- Required updates when dependencies or runtime change:
- Next environment improvement:
