# BUG-002: Session does not expire after configured idle timeout

**Severity:** Medium
**Priority:** P1
**Environment:** Staging, Firefox 128, Windows 11
**Found during:** TC-07 execution

## Summary
The application is configured with a 15-minute idle session timeout (per staging config), but sessions remain active well past this window. Tested at 30 minutes idle with no forced logout.

## Steps to Reproduce
1. Log in successfully
2. Leave the browser tab idle (no interaction) for 30 minutes
3. Return and attempt an action that requires authentication (e.g., viewing account settings)

## Expected Result
After 15 minutes of inactivity, the session should expire; the next authenticated action should redirect to login with a "session expired" message.

## Actual Result
The action succeeds normally at 30 minutes idle — no redirect, no expiry message. Session appears to persist indefinitely, or the timeout is not being enforced client-side.

## Impact
On shared or public devices, this extends the window during which an unattended, logged-in session could be accessed by someone else — a security concern beyond just a UX inconsistency.

## Notes for Investigation
Worth checking whether the timeout is enforced server-side (token expiry) vs. only intended to be enforced client-side (inactivity timer) — the fix differs significantly depending on which layer is missing the check.
