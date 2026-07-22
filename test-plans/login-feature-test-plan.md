# Test Plan: User Login Feature

## 1. Objective
Verify that the login feature correctly authenticates valid users, rejects invalid attempts, and handles edge cases (locked accounts, empty fields, session expiry) without exposing security or usability issues.

## 2. Scope

**In scope:**
- Standard email/password login
- Field validation (empty, malformed input)
- Account lockout after repeated failed attempts
- Session persistence and expiry
- "Forgot password" entry point (link only, not the reset flow itself)

**Out of scope:**
- Password reset flow (covered in a separate test plan)
- Third-party SSO login (not yet implemented)

## 3. Test Approach

| Layer | Approach |
|---|---|
| Unit | Owned by dev team |
| API | Automated (see `api-testing-framework` repo) — validates auth endpoint contracts |
| UI | Automated smoke tests (see `orangehrm-selenium-automation` repo) + manual exploratory testing |
| Security | Manual — brute-force/lockout behavior, SQL injection spot-checks on input fields |

## 4. Entry / Exit Criteria

**Entry:** Feature deployed to staging; API auth endpoints stable.
**Exit:** All P0/P1 test cases pass; no open Critical or High severity bugs; automated smoke suite green in CI.

## 5. Risks

- Lockout logic depends on a shared rate-limiting service — flagged for extra edge-case coverage since a misconfiguration there could lock out legitimate users.
- No dedicated staging data for "locked account" state — requires either seeded test data or manual setup before test execution.

## 6. Deliverables
- Executed test case results (see `test-cases/`)
- Bug reports for any failures (see `bug-reports/`)
- Automated regression suite integrated into CI
