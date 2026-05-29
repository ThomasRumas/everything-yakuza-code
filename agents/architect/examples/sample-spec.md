# Business Specification

## Business Context
- Problem: Users cannot reset their password when locked out of their account
- Goal: Add a password reset flow via email verification
- Users: Registered users who have forgotten their password
- Expected Outcome: User receives a reset email, clicks the link, sets a new password, and can log in

## Business Acceptance Criteria (BDD)

Scenario 1: User requests a password reset
Given a user with a registered email address
When they submit the forgot-password form
Then a reset email is sent with a unique token valid for 1 hour

Scenario 2: User resets their password
Given a valid, non-expired reset token
When the user submits a new password
Then the password is updated and the token is invalidated

Scenario 3: Expired token is rejected
Given a reset token older than 1 hour
When the user attempts to use it
Then a 400 error is returned and the user is asked to request a new link

## Edge Cases
- Case 1: Email not found — return 200 (do not reveal whether email exists)
- Case 2: Token already used — return 400 with message "token already used"
- Case 3: New password same as old — return 400 with message "choose a different password"

## Risk Assessment

| Severity | Risk | Mitigation |
|----------|------|------------|
| HIGH | Token guessable | Use cryptographically secure random 32-byte hex token |
| MEDIUM | Token not invalidated after use | Delete token from DB on successful reset |
| LOW | Email delivery failure | Log failure, return 200 to avoid email enumeration |

---

# Technical Specification

## Architecture Overview
- Components: `POST /auth/forgot-password`, `POST /auth/reset-password`, `reset_tokens` DB table, email service
- Data Flow: Request → generate token → store in DB → send email → user clicks link → validate token → update password → delete token
- External Dependencies: nodemailer (email), bcrypt (password hashing)

## Technical Constraints
- Language: TypeScript
- Framework: Express.js
- Runtime: Node.js 20
- Performance: Email send must not block the HTTP response — fire and forget
- Security: Tokens must be single-use, time-limited, and stored hashed

## Design Decisions

Decision 1:
- Choice: Store token as SHA-256 hash in DB, send raw token in email
- Reason: If DB is compromised, tokens cannot be used directly
- Alternatives Considered: Store raw token (rejected — security risk)

---

# Implementation Plan

## Task List (Atomic & Ordered)

### Phase 1: Database

Task 1:
- Type: CREATE_FILE
- Path: src/db/migrations/004_reset_tokens.sql
- Details:
  - Create table `reset_tokens` with columns: id, user_id (FK), token_hash (VARCHAR 64), expires_at (TIMESTAMP), used_at (TIMESTAMP nullable)

Task 2:
- Type: CREATE_FILE
- Path: src/db/resetTokenRepository.ts
- Details:
  - Export `createToken(userId: string, tokenHash: string, expiresAt: Date): Promise<void>`
  - Export `findToken(tokenHash: string): Promise<ResetToken | null>`
  - Export `markTokenUsed(tokenHash: string): Promise<void>`

### Phase 2: Service

Task 3:
- Type: CREATE_FILE
- Path: src/services/passwordResetService.ts
- Details:
  - Export `requestReset(email: string): Promise<void>` — look up user, generate 32-byte hex token, hash with SHA-256, store, send email, return void regardless of whether email exists
  - Export `resetPassword(token: string, newPassword: string): Promise<void>` — hash token, find in DB, validate expiry and used_at, hash new password with bcrypt, update user, mark token used

### Phase 3: Routes

Task 4:
- Type: CREATE_FILE
- Path: src/routes/auth.ts
- Details:
  - `POST /auth/forgot-password` — body `{ email }`, call `requestReset`, always return 200
  - `POST /auth/reset-password` — body `{ token, password }`, call `resetPassword`, return 200 or 400

---

## Complexity Estimation

| Area | Estimate |
|------|----------|
| Database | 1–2 hours |
| Service | 2–3 hours |
| Routes | 1 hour |
| **Total** | **4–6 hours** |

Complexity: MEDIUM

---

## File Dependency Graph

004_reset_tokens.sql -> resetTokenRepository.ts -> passwordResetService.ts -> auth.ts

---

## Interfaces & Contracts

Interface: ResetToken
Fields:
- id: string
- userId: string
- tokenHash: string
- expiresAt: Date
- usedAt: Date | null

---

# Testing Strategy

## Unit Tests
- File: src/services/passwordResetService.test.ts
- Cases:
  - Returns void if email not found (no error thrown)
  - Throws if token expired
  - Throws if token already used
  - Updates password and marks token used on success

## Integration Tests
- Flow: POST /auth/forgot-password → check DB for token → POST /auth/reset-password → verify login works

## Mocking Strategy
- Mock nodemailer transport in unit tests
- Use test DB in integration tests

---

# Definition of Done

- [ ] Code compiles
- [ ] Lint passes
- [ ] Tests pass
- [ ] No TODO comments
- [ ] All acceptance criteria satisfied

---

# Assumptions

1. A `users` table with `id`, `email`, and `password_hash` columns already exists
2. nodemailer is already installed and configured via environment variables
3. bcrypt is already installed

---

WAITING FOR CONFIRMATION: Proceed with this plan? (yes / modify: <changes> / different approach: <alternative> / skip phase N)
