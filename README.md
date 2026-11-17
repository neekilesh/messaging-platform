# NEXORA

**Your Identity. Your World. Your Connections.**

NEXORA is a secure, username-based social messaging platform designed as a production-oriented monorepo (Next.js web + NestJS API + PostgreSQL + Redis + S3-compatible encrypted media storage) with real-time communication and mandatory end-to-end encryption goals.

> **Status:** Early foundation/scaffold in progress. Many features described in `master promt.txt` are not yet implemented. Do not treat this repository as production-ready.

## Product Scope (Planned)
- Unique username accounts with email OTP verification (no phone number)
- Private 1:1 messaging, groups (max 25 members), voice messages, photo/video sharing
- 24-hour stories, friend streaks, disappearing messages
- Real-time delivery/read/typing/presence via WebSockets
- End-to-end encryption (audited protocols only; no homemade crypto)
- Magical/futuristic UI with accessibility and reduced-motion support
- **Not included:** voice/video calls, camera filters

## Repository Structure
- `apps/web` — Next.js frontend
- `apps/api` — NestJS backend API & realtime gateways
- `packages/*` — shared types/validation/crypto/ui/config packages
- `database/` — migrations, seeds, schema artifacts
- `infrastructure/` — docker/nginx/scripts/deployment assets
- `docs/` — architecture, API, security, encryption, deployment docs
- `tests/` — integration/e2e/security/performance test suites

## Prerequisites
- Node.js 20+
- pnpm 9+
- Docker & Docker Compose
- PostgreSQL 16+ and Redis 7+ (or use Docker Compose)

## Setup
1. **Install dependencies**
   ```bash
   pnpm install
   ```
2. **Configure environment**
   ```bash
   cp .env.example .env
   # Edit .env with real values (DB, Redis, email/SMTP, S3-compatible storage, secrets)
   ```
3. **Start infrastructure (PostgreSQL + Redis)**
   ```bash
   docker compose up -d postgres redis
   ```
4. **Run database migrations** (once Prisma schema/migrations are finalized)
   ```bash
   pnpm --filter @nexora/api prisma:migrate
   ```
5. **Configure email verification**
   - Set `SMTP_*` / provider variables in `.env` for transactional OTP delivery.
6. **Start backend**
   ```bash
   pnpm --filter @nexora/api dev
   ```
7. **Start frontend**
   ```bash
   pnpm --filter @nexora/web dev
   ```

## Testing
```bash
pnpm test
```
(Additional integration/e2e/security/performance suites will be added under `tests/` and app packages.)

## Build for Production
```bash
pnpm build
```

## Deployment
- Prepare production PostgreSQL, Redis, S3-compatible encrypted media storage, SMTP/email provider, HTTPS/reverse proxy, monitoring, and backups.
- Use `docker-compose.yml` and `infrastructure/` assets as a starting point (production hardening required).
- See `docs/DEPLOYMENT.md` and `docs/SECURITY.md` for requirements and checklists (to be expanded).

## Documentation
- `docs/ARCHITECTURE.md`, `docs/DATABASE.md`, `docs/API.md`, `docs/SECURITY.md`, `docs/ENCRYPTION.md`, `docs/DEPLOYMENT.md`, `docs/DEVELOPMENT.md`
- `PROJECT_STATUS.md` — current implementation and security status
- `NEXORA_MASTER_ANALYSIS.md` — structured analysis of the master prompt

## Security Notice
End-to-end encryption is a mandatory product requirement but **must be implemented using audited protocols/libraries and verified by tests/review** before any E2EE claims. This scaffold does not constitute a completed or audited secure implementation.
