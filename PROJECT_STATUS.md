# NEXORA Project Status

_Last updated: continued implementation (backend/API foundations expanded; environment/install blockers persist)_

## Summary
- **Phase:** 1 — Project foundation (scaffold created, hardening pending)
- **Repository state:** Monorepo scaffold established with root tooling, docs, Docker Compose, initial web/API app skeletons, Prisma schema draft, and shared package stubs. Core product features are **not yet implemented** and E2EE is **not implemented**.

## Completed
- [x] Workspace inspected and master prompt analyzed (`NEXORA_MASTER_ANALYSIS.md`, `master promt.txt`)
- [x] Monorepo directory structure scaffolded (`apps`, `packages`, `infrastructure`, `database`, `docs`, `tests`)
- [x] Root configs/scaffolds: `package.json`, `pnpm-workspace.yaml`, `turbo.json`, `.gitignore`, `.env.example`, `.prettierrc`, `docker-compose.yml`
- [x] Documentation scaffolds: `README.md`, `docs/{ARCHITECTURE,DATABASE,API,SECURITY,ENCRYPTION,DEPLOYMENT,DEVELOPMENT}.md`
- [x] **Web app scaffold (`apps/web`)**: Next.js + TypeScript + Tailwind config, cosmic theme/globals, app layout and initial routes/pages (home, welcome, auth login/register placeholders, dashboard, chats, groups, stories, profile, settings)
- [x] **API app scaffold (`apps/api`)**: NestJS bootstrap, global validation/CORS/prefix, health endpoint (`GET /api/v1/health`), Prisma module/service placeholders, `nest-cli.json`, TS configs
- [x] **Database draft**: initial Prisma schema (`apps/api/prisma/schema.prisma`) covering core entities (users/profiles/verifications/sessions/devices/keys, conversations/messages/receipts/reactions/attachments, groups/members/roles/encryption state, stories/views/reactions, friendships/requests/blocks/streaks, notifications, settings, reports)
- [x] **Shared package stubs**: `packages/types`, `packages/validation` (initial constants/types placeholders)

## In Progress
- [~] Phase 1 hardening: dependency install, runnable dev servers, lint/typecheck/build verification, Prisma client/migration baseline
  - [x] Added npm workspace compatibility (`workspaces` in root `package.json`) and fixed auth route links/placeholders (`/login`, `/register`, `/forgot-password`, `/reset-password`, `/verify-otp`, `/username`, `/splash`)
  - [x] Added missing scaffold configs (`apps/web/.eslintrc.json`, `apps/api/tsconfig.build.json`, `packages/validation/tsconfig.json`, basic shared/config package stubs)
  - [x] Created `.env` from `.env.example`
  - [x] **Dependencies installed successfully** with `npx pnpm@9.12.0 install` using `.npmrc` (`node-linker=hoisted`, `shamefully-hoist=true`) after Developer Mode enabled; `pnpm-lock.yaml` generated
  - [x] **Typechecks pass**: `apps/api` and `apps/web` (`node node_modules/typescript/bin/tsc -p <tsconfig> --noEmit`)
  - [x] **Prisma Client regenerated** against `apps/api/prisma/schema.prisma`
  - [ ] Dev server/build runtime verification, Prisma migration baseline generated/applied against a running PostgreSQL database
- [~] Phase 3/4/5 groundwork (in progress):
  - **Auth (implemented in code, pending runtime verification/config):** DTOs/endpoints for request OTP, verify OTP, register, login, forgot/reset password, authenticated logout, `GET /auth/me`, username availability; username normalization/validation utils; OTP generation/hash/verification and session token hashing utils; Argon2id password hashing/verification utils; Prisma-backed flows for registration OTP issue/verify, user registration (email/username uniqueness, OTP consumption, profile/privacy/session creation), login, password reset request/reset with session revocation; resend cooldown checks and anti-enumeration response shape for reset requests; Bearer-token `SessionAuthGuard` + `CurrentUser` decorator for session context. Email delivery remains scaffolded (provider integration required); rate limiting/brute-force guards, secure cookie/device binding, token rotation/CSRF strategy, and full security hardening still pending.
  - **Users:** search endpoint stub
  - **Conversations (private 1:1, partial implementation):** auth-guarded endpoints/service for create-or-get direct conversation, list conversations, list messages (cursor/limit), send message, edit message (sender-only), delete message (sender-only soft-delete/clear ciphertext), and react to message (participant). Realtime delivery/receipts/typing/presence, chat requests flow, forward/copy/pin, blocks/report, attachments/media handling, and full sync semantics still pending.
  - **Groups (partial implementation):** auth-guarded endpoints/service for list, create (owner+members, role rows, transactional), add member with **backend-enforced 25-member cap** and owner/admin permission checks, rejoin handling, and group message list/send (membership-checked, cursor pagination, stores ciphertext). Group encryption state initialization/rotation, remove/leave/delete/roles management, realtime delivery/receipts, and full settings still pending.
  - **Realtime:** `ChatGateway` scaffold wired (no authenticated messaging/presence logic yet)
- [ ] Design system foundations (reusable UI primitives, layout shells, animation tokens, accessibility patterns)

## Additional Backend Progress (current iteration)
- **Friends (`/api/v1/friends`)**: send/list/accept/reject friend requests with Prisma-backed logic (partial)
- **Stories (`/api/v1/stories`)**: create (24h expiry), list active, mark view (partial; privacy/friend filtering and E2EE media pipeline pending)
- **Streaks (`/api/v1/streaks`)**: list streaks for user (partial; server-side activity/streak calculation engine, reminders/milestones pending)
- **Groups**: added group details, remove member/leave, and group message list/send endpoints (partial)

## Pending / Not Completed (project not fully complete)
- **Phase 1 hardening:** full dependency install/link verification on Windows (EPERM symlink blocker), confirmed web/api dev+build+lint+typecheck, Prisma migrations generated/applied against running DB
- **Phase 2:** complete magical UI/UX for all required screens with real state/navigation/API integration, animations, accessibility/reduced-motion, responsive behavior
- **Phase 3:** real email provider delivery, rate limiting/brute-force/OTP abuse protections, secure cookie/device binding, CSRF/token rotation/session hardening, full account/device management flows
- **Phase 4:** chat request UX/flow integration, realtime Socket.IO authenticated events (messages/receipts/typing/presence), forward/copy/pin, blocks/report enforcement, attachments, robust sync/pagination/ordering/idempotency
- **Phase 5:** full group admin (remove/promote/demote/delete/info/settings), group realtime, audited group E2EE/key lifecycle on membership changes, comprehensive 25-member enforcement tests
- **Phase 6:** voice recording/playback/waveforms, photo/video/document upload with client-side encryption before upload, encrypted S3-compatible storage, previews/thumbnails, authorized short-lived downloads
- **Phase 7:** complete stories privacy/viewers/reactions/replies, disappearing messages engine/cleanup/retention, streak rules engine/reminders/milestones, notifications system
- **Phase 8 (modified by user request):** E2EE deferred. Do not claim E2EE; focus on standard security hardening (auth/session/authorization/rate limiting/input validation/secure access controls). If required later, implement audited Signal/MLS, key lifecycle, encrypted media pipeline, and verification.
- **Phase 9:** automated tests (unit/integration/e2e/security/performance) for all required scenarios, bug fixes, performance/responsive/security verification
- **Phase 10:** production Docker/HTTPS/reverse proxy, managed DB/Redis/storage/email config, monitoring/backups/CI-CD, deployment, finalized docs and delivery artifacts

**Reality check:** Full project completion (as specified in `master promt.txt`) requires external services/credentials, successful runtime environment (DB/Redis/storage/email), audited cryptographic integrations, extensive testing, and deployment verification that cannot be fully completed in this environment/session. Work has continued with real source-code implementations where feasible; blockers and remaining required work are documented honestly.

## Known Issues / Blockers
- **Docker not installed** on this machine (cannot start PostgreSQL/Redis/MinIO via compose yet). Need Docker running or external `DATABASE_URL`/`REDIS_URL`.
- **Prisma migrations not generated/applied**; schema is draft and requires refinement/review before baseline migration.
- No real **SMTP/email provider**, **S3-compatible storage**, or production secrets configured (developer configuration required).
- **E2EE explicitly deferred by user request:** end-to-end encryption (Signal/MLS/audited protocols, encrypted media pipeline, key/device lifecycle) is not being implemented at this time. Do not claim E2EE; security architecture must be revisited if E2EE is later required. Transport/storage security and standard app protections still need proper implementation.
- Many product features remain incomplete (realtime delivery/receipts/typing/presence, full chat/group admin flows, media/voice encrypted pipeline, stories/streaks/disappearing/notifications behavior, full UI integration, tests, deployment).
- Dev server/build runtime verification and automated tests not yet executed.

## Latest Hardening Notes
- Node `v22.14.0`, npm `11.1.0`, pnpm used via `npx pnpm@9.12.0`.
- Developer Mode enabled (user-confirmed); pnpm install succeeded with hoisted linker config (`.npmrc`) and produced `pnpm-lock.yaml`.
- `apps/api` and `apps/web` TypeScript checks pass; Prisma Client regenerated.
- Next access needed to continue end-to-end: Docker running or external DB/Redis, SMTP credentials, S3/MinIO config, and (if applicable) deployment target/access.

## Database Migration Status
- Initial Prisma schema draft: **created** (`apps/api/prisma/schema.prisma`)
- Migrations: **not generated**
- Applied to database: **no**

## Security Implementation Status
- Password hashing, OTP issuance/verification, session management, rate limiting, authorization enforcement: **not implemented**
- E2EE (private/group/media), device/key management, encrypted storage access controls: **not implemented**
- Security and abuse-resistance testing: **not implemented**

## Latest Test Results
- Not run. No automated test results available yet.
