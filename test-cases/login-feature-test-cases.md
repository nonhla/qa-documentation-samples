# Test Cases: User Login Feature

| ID | Title | Preconditions | Steps | Expected Result | Priority |
|---|---|---|---|---|---|
| TC-01 | Successful login with valid credentials | User has an active account | 1. Navigate to login page 2. Enter valid email + password 3. Click "Log in" | User is redirected to dashboard; session is created | P0 |
| TC-02 | Login fails with incorrect password | User has an active account | 1. Enter valid email 2. Enter incorrect password 3. Click "Log in" | Generic "invalid email or password" error shown; no indication of which field was wrong | P0 |
| TC-03 | Login fails with unregistered email | — | 1. Enter an email not in the system 2. Enter any password 3. Click "Log in" | Same generic error as TC-02 (prevents user enumeration) | P0 |
| TC-04 | Empty fields are rejected client-side | — | 1. Leave both fields empty 2. Click "Log in" | Inline validation errors appear; no network request is sent | P1 |
| TC-05 | Account locks after 5 failed attempts | User has an active account | 1. Attempt login with wrong password 5 times in a row | On the 5th attempt, account is locked and user sees a lockout message with a way to unlock (e.g., email link) | P0 |
| TC-06 | Session persists across page refresh | User is logged in | 1. Refresh the page | User remains logged in | P1 |
| TC-07 | Session expires after configured timeout | User is logged in | 1. Leave session idle past the timeout window 2. Attempt an action | User is redirected to login with a "session expired" message | P1 |
| TC-08 | Password field masks input | — | 1. Type a password into the password field | Characters are masked (dots/asterisks); a show/hide toggle works if present | P2 |
| TC-09 | "Forgot password" link navigates correctly | — | 1. Click "Forgot password" | User is taken to the password reset entry point | P2 |
| TC-10 | SQL injection attempt in login fields is rejected safely | — | 1. Enter `' OR '1'='1` in email and password fields 2. Submit | Login fails with standard invalid-credentials error; no error stack trace or DB info is exposed | P0 |

**Notes:**
- P0 = must pass before release. P1 = should pass, can ship with a tracked known issue if needed. P2 = nice-to-have coverage.
- TC-10 exists because auth forms are a common injection target — worth explicit coverage even in a "functional" test pass.
