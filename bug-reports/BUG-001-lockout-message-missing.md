# BUG-001: No lockout message shown after 5 failed login attempts

**Severity:** High
**Priority:** P0
**Environment:** Staging, Chrome 126, macOS 14
**Found during:** TC-05 execution

## Summary
After 5 consecutive failed login attempts with an incorrect password, the account is correctly locked on the backend (verified via API — subsequent correct-password attempts also fail), but the UI shows the same generic "invalid email or password" error instead of a distinct lockout message. Users have no way to know their account is locked versus just mistyping their password again.

## Steps to Reproduce
1. Go to the login page
2. Enter a valid email with an incorrect password
3. Submit, repeat 5 times total
4. On the 5th (and any subsequent) attempt, observe the error message

## Expected Result
After the account locks, the UI should show a distinct message (e.g., "Your account has been locked due to multiple failed attempts. Check your email to unlock it.") rather than the standard invalid-credentials error.

## Actual Result
The generic "invalid email or password" message is shown every time, identical to a normal wrong-password error.

## Impact
Users who get locked out have no way to distinguish "I mistyped my password" from "my account is locked," so they'll keep retrying — potentially triggering additional lockout-related backend logic — instead of taking the correct recovery action.

## Suggested Fix Area
Backend likely already returns a distinct status/error code for the locked state (confirmed via API response); the frontend appears to be catching all 401-type responses with one generic message rather than branching on the lockout-specific case.
