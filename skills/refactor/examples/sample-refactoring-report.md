## Refactoring Report

### Summary
- Critical Issues: 1
- Major Issues: 1
- Minor Issues: 2

### Detailed Findings

#### Issue 1
- Category: Critical
- File: src/services/passwordResetService.ts
- Location: requestReset(), line 18
- Problem: Token expiry is hardcoded as `new Date(Date.now() + 3600000)` — magic number with no explanation
- Proposed Change: Extract to named constant `const TOKEN_TTL_MS = 60 * 60 * 1000 // 1 hour`
- Rationale: Magic numbers hide intent and make TTL changes error-prone
- Safety Check:
  - Behavior Preserved: YES
  - Atomic: YES
  - Testable: YES
  - Risk Level: LOW
- Validation: Run unit tests — token expiry behavior unchanged

#### Issue 2
- Category: Major
- File: src/services/passwordResetService.ts
- Location: resetPassword(), line 42
- Problem: Token lookup and validation are in the same function — hard to unit test validation logic in isolation
- Proposed Change: Extract `validateToken(token: ResetToken): void` that throws if expired or used
- Rationale: Single responsibility, enables testing validation independently of DB calls
- Safety Check:
  - Behavior Preserved: YES
  - Atomic: YES
  - Testable: YES
  - Risk Level: LOW
- Validation: Existing integration tests still pass after extraction

#### Issue 3
- Category: Minor
- File: src/routes/auth.ts
- Location: POST /auth/reset-password handler
- Problem: Error message leaks internal error class name: `error.message` may expose `TokenExpiredError`
- Proposed Change: Map known error types to user-facing messages in a `toHttpError()` helper
- Rationale: Avoids leaking implementation details to clients
- Safety Check:
  - Behavior Preserved: YES
  - Atomic: YES
  - Testable: YES
  - Risk Level: LOW
- Validation: Test that 400 response body contains only `{ error: "..." }` with a safe message

#### Issue 4
- Category: Minor
- File: src/db/resetTokenRepository.ts
- Location: findToken()
- Problem: Function returns `null` when token not found — callers must check for null without a clear contract
- Proposed Change: Add JSDoc `@returns ResetToken if found, null if not found` comment
- Rationale: Documents the null case explicitly for future maintainers
- Safety Check:
  - Behavior Preserved: YES
  - Atomic: YES
  - Testable: YES
  - Risk Level: LOW
- Validation: No behavior change — documentation only

### Proposed Refactoring Plan

Step 1:
- File: src/services/passwordResetService.ts
- Change: Extract `TOKEN_TTL_MS` constant at module level, replace magic number on line 18

Step 2:
- File: src/services/passwordResetService.ts
- Change: Extract `validateToken(token: ResetToken): void` private function, call it from `resetPassword`

Step 3:
- File: src/routes/auth.ts
- Change: Add `toHttpError(err: unknown): string` helper, use it in the reset-password error handler

Step 4:
- File: src/db/resetTokenRepository.ts
- Change: Add JSDoc comment to `findToken` documenting the null return

### Rejected Suggestions

Suggestion 1:
- Reason for Rejection: Switching from SHA-256 to bcrypt for token hashing would change security properties and is a HIGH risk architectural change
- Risk: HIGH — behavior and performance impact

### Risk Assessment
- Overall Risk Level: LOW
- Potential Impacts: No behavior changes in Steps 1–4
- Regression Risk: Very low — all changes are pure refactors with no logic modification

### Compliance Check
- Aligns with Architect Plan: YES
- Violations Detected: None

### Approval Required

No changes have been applied. User must explicitly approve before any modification.
