## Execution Report

### Completed Tasks
- Task 1: DONE — created src/db/migrations/004_reset_tokens.sql
- Task 2: DONE — created src/db/resetTokenRepository.ts
- Task 3: DONE — created src/services/passwordResetService.ts
- Task 4: DONE — created src/routes/auth.ts

### Modified / Created Files
- src/db/migrations/004_reset_tokens.sql
- src/db/resetTokenRepository.ts
- src/services/passwordResetService.ts
- src/routes/auth.ts

### Acceptance Criteria Validation

Scenario 1: User requests a password reset
- Status: PASS
- Notes: requestReset generates a 32-byte hex token, hashes with SHA-256, stores in DB, sends email, returns void

Scenario 2: User resets their password
- Status: PASS
- Notes: resetPassword validates expiry and used_at, updates password hash, calls markTokenUsed

Scenario 3: Expired token is rejected
- Status: PASS
- Notes: Service throws TokenExpiredError when expiresAt < now; route returns 400

### Issues Encountered
- None
