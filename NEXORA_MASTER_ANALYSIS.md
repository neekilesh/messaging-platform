# NEXORA — Master Prompt Analysis (Structured)

**Source:** `master promt.txt`  
**Application:** NEXORA  
**Tagline:** Your Identity. Your World. Your Connections.  
**Category:** Secure social messaging application

---

## 1. Executive Summary

NEXORA is a production-oriented, username-based, end-to-end encrypted messaging platform with a distinctive futuristic/"magical" UI. It combines private messaging, groups (max 25 members), voice messages, photo/video sharing, 24-hour stories, friend streaks, and disappearing messages. **No voice/video calls, no camera filters.** The goal is a complete, maintainable product (frontend, backend, database, realtime, security, deployment)—not a prototype or WhatsApp clone.

---

## 2. Agent Role & Delivery Expectations

| Aspect | Expectation |
|---|---|
| **Role** | Senior architect, full-stack, security, DB, UI/UX, animation, QA |
| **Approach** | Systematic, clean codebase, proper file management, working features (not placeholders) |
| **Scope** | Complete app: frontend + backend + DB + auth + realtime + security + deployment |
| **Honesty** | Report blockers/incomplete items; never fake implementations or claim E2EE without real implementation/tests |
| **Completion bar** | Do not stop at plan/structure; continue implementing within environment |

---

## 3. Product Vision & Identity

- **Experience:** Futuristic digital universe; immersive, polished, memorable interactions.
- **Inspiration (functional):** WhatsApp-style private chat, Snapchat-inspired stories/streaks, modern groups.
- **Differentiator:** Extraordinary magical interface + strong E2EE as core (not optional).

---

## 4. Strict Product Requirements (Mandatory)

| Requirement | Specification |
|---|---|
| Account identity | Unique username |
| Registration verification | Gmail/email OTP |
| Phone number | Not required |
| Private messaging | One-to-one |
| Group messaging | Up to **25 members per group** (including creator) |
| Voice messages | Supported |
| Photo sharing | Supported |
| Video sharing | Supported |
| Stories | 24-hour stories |
| Streaks | Friend streak system |
| Disappearing messages | Supported |
| End-to-end encryption | Mandatory |
| Voice calls | **Not included** |
| Video calls | **Not included** |
| Camera filters | **Not included** |
| Magical interface | Mandatory |

**Constraints:** >25 registered users supported overall; per-group cap = 25 enforced on backend. No out-of-scope features.

---

## 5. Core Functional Specifications

### 5.1 Registration & Authentication
- **Flow:** Open → Create Account → Email → 6-digit OTP → Verify → Unique username → Display name → Strong password → Optional avatar → Created.
- **Username:** Unique, searchable, controlled changes, availability check, normalized uniqueness field; discoverable without exposing email.
- **Security:** Argon2id hashing, short-lived/expiring OTP, resend cooldown, rate limiting, brute-force protection, secure sessions, verified-email password reset, logout, device/session management.
- **Email:** Transactional email/SMTP provider (not personal Gmail as auth DB). Never store plaintext passwords/OTPs.

### 5.2 Private One-to-One Messaging
- **Capabilities:** Search by username, view profiles, chat requests (accept/reject), realtime text/media/voice/docs, edit/delete/reply/react/forward/copy/pin, conversation search/mute/archive, block/report.
- **Presence & receipts:** Typing indicators, online/offline, delivery/read receipts.
- **Statuses:** Sending → Sent → Delivered → Read → Failed (meaningful visuals).
- **Sync:** Across authorized user devices.

### 5.3 Group Messaging (≤25 members)
- **Creation:** Name, picture, add by usernames, description, privacy settings.
- **Features:** Realtime messaging, voice/photo/video, replies/reactions/forwarding, search, member list, notifications, media gallery, disappearing messages, add/remove/leave/delete (authorized).
- **Roles:** Owner (full control), Administrator (moderate/manage per permissions), Member (participate/view).
- **Security/keys:** Backend-enforced limit; secure group key management on membership changes; new members must not auto-access prior protected history unless explicitly authorized.

### 5.4 Voice Messaging
- Press-and-hold record, swipe cancel, release send; playback controls (pause/resume/speed), duration, reactive waveform visualization, recording animation, delivery status. **No audio/video calling.**

### 5.5 Photo & Video Sharing
- Select/upload/send (private & group), previews/full-screen/playback/thumbnails/duration, permitted downloads, image compression before encryption+upload.
- **Media security:** Encrypt before upload, keys separate from stored media, authenticated encryption, short-lived download authorization, no public URLs to private content, validate type/size, prevent unauthorized access, clear progress/error handling.

### 5.6 Stories (24h, E2EE)
- Photo/video stories, auto-expire 24h, friends’ stories, view progress, owner-visible viewers, reactions, replies via private DM, visibility/privacy controls, delete.
- **UI:** Circular previews with animated progress rings; viewed vs unviewed states; smooth open/close transitions.

### 5.7 Friend Streaks
- Per-friend streaks with count/flame icon, reset on missed activity window, reminders/milestones, shown in conversations.
- **Rule:** Consistent qualifying reciprocal activity (e.g., photo/video/message) within 24h window.
- **Integrity:** Server-side timestamps; no client-side manipulation.

### 5.8 Disappearing Messages
- Options (suggested): 10s, 1m, 1h, 24h, 7d, 30d; conversation-level settings as appropriate.
- Server-side expiration/cleanup; disclose screenshot/recording/copy limitations.
- Delete expired content and encryption material per retention policy (E2EE-aware).

---

## 6. Magical UI/UX & Interaction Design

### 6.1 Visual Direction
- Deep cosmic dark backgrounds, purple/midnight blue base, neon violet/electric blue/cyan accents.
- Glassmorphism, translucent panels, soft gradients, glowing borders, floating elements, smooth transitions, elegant typography.
- Readable and usable; avoid clutter.

### 6.2 Key Interaction Requirements
- **Open conversation:** Animated card expansion, smooth reveal, subtle particles/lighting, consistent navigation.
- **Sending:** Animated bubbles/entrances, delivery states, animated reactions, visually connected replies.
- **Voice:** Animated recording UI, reactive waveforms, animated playback progress.
- **Media:** Cinematic transitions, smooth expand, immersive video, accessible controls.
- **Stories:** Animated rings, fluid transitions, beautiful progress.
- **Navigation:** Smooth page transitions, animated selection, subtle background motion.

### 6.3 Animation & Accessibility
- Suggested tech: Framer Motion, Lottie, Rive, CSS/appropriate native APIs.
- Animations must improve interaction (not decorative only). **Support reduced-motion preferences.**

---

## 7. Application Screens (Working Navigation Required)

| Area | Screens |
|---|---|
| **Authentication** | Splash, Welcome, Login, Registration, Email OTP verification, Username selection, Forgot password, Password reset |
| **Main application** | Home dashboard, Private chat list, One-to-one conversation, Group conversation, Create group, Group information, User profile, User search, Friend requests, Stories dashboard, Story viewer, Media gallery, Voice message interface, Notifications, Settings, Privacy settings, Security settings, Blocked users, Device/session management |

**Rule:** No dead buttons or nonfunctional interactions.

---

## 8. Technical Architecture

### 8.1 Recommended Stack
| Layer | Technologies |
|---|---|
| Frontend | Next.js, React, TypeScript, Tailwind CSS, Framer Motion, Rive/Lottie (as appropriate) |
| Backend | Node.js, NestJS, TypeScript, REST API, WebSockets (Socket.IO) |
| Database | PostgreSQL, Prisma ORM |
| Cache/Jobs | Redis, BullMQ |
| Media storage | S3-compatible object storage (encrypted upload/retrieval) |
| Auth | Email OTP, Argon2id, secure session management |
| Encryption | Audited Signal Protocol impl (private), audited group protocol e.g. MLS (groups); **no homemade crypto** |
| Deployment | Docker, Docker Compose, env-based config, production reverse proxy, CI/CD |

**Modularity:** Design so native Android/iOS can be added later without rebuilding backend.

### 8.2 Real-time & API
- **REST modules (suggested):** `/api/v1/{auth,users,friends,conversations,messages,groups,stories,streaks,media,notifications,devices}`
- **WebSockets for:** Realtime messages, delivery acks, read receipts, typing, online status, group updates, notifications.
- **Requirements:** Authenticated/authorized connections, reconnect handling, duplicate protection, ordering, reliable acks, rate limiting, proper errors. **Never trust frontend authorization.**

---

## 9. End-to-End Encryption (Critical)

- **Private chats:** Audited Signal Protocol (or compatible) — identity/prekeys, session establishment, forward secrecy, post-compromise security (where supported), authenticated encryption, device-specific keys, rotation, secure recovery design.
- **Groups:** Audited group protocol (e.g. MLS) — secure establishment, membership changes, key updates, member removal, secure state sync, protection against unauthorized historical access.
- **Media:** Encrypt locally before upload, secure key generation, encrypted-only storage, share keys via encrypted conversations, integrity verification.
- **Server boundary:** No access to plaintext messages/keys/decrypted media; store only encrypted payloads + strictly necessary delivery/operational metadata. Acknowledge metadata limits (sender/timestamps/membership/IPs may be visible).
- **Assurance:** Do not claim E2EE until implemented & tested; provide security architecture, threat model, key lifecycle, independent review plan.

---

## 10. Database Design (High-Level)

- **DB:** Normalized PostgreSQL, UUIDs, FKs, indexes, timestamps (`created_at`, `updated_at`), safe deletes/retention, pagination/high-volume design, no plaintext encrypted content stored.
- **Enforce:** Username uniqueness, group member limits transactionally.
- **Models (indicative):** User, UserProfile, EmailVerification, UserSession, Device, IdentityKey, PreKey, PrivateConversation, ConversationParticipant, Message, MessageReceipt, MessageReaction, MessageAttachment, Group, GroupMember, GroupRole, GroupEncryptionState, Story, StoryView, StoryReaction, FriendRequest, Friendship, UserBlock, Streak, Notification, DisappearingMessageSetting, UserPrivacySetting, Report.
- **Migrations:** Version-controlled; never manual prod schema changes without migration.

---

## 11. Project Structure (Recommended Monorepo)

```text
NEXORA/
├── apps/
│   ├── web/                 # Next.js frontend (public, src/app, components, features, hooks, services, stores, lib, styles, types, tests)
│   └── api/                 # NestJS backend (modules, common, config, database, gateways, jobs, main.ts, tests)
├── packages/                # shared, types, validation, crypto, ui, config
├── infrastructure/          # docker, nginx, scripts, deployment
├── database/                # migrations, seeds, schema
├── docs/                    # ARCHITECTURE, DATABASE, API, SECURITY, ENCRYPTION, DEPLOYMENT, DEVELOPMENT
├── tests/                   # integration, e2e, security, performance
├── .env.example
├── .gitignore
├── docker-compose.yml
├── package.json
├── pnpm-workspace.yaml
├── turbo.json
└── README.md
```

Adjust only as necessary while preserving clear separation of concerns.

---

## 12. File Management & Engineering Discipline

- Inspect before writing; never overwrite/delete without inspection/need.
- Modular code: small reusable components, separated business/API/DB logic, shared types in packages.
- No secrets in source; `.env` excluded, `.env.example` provided; migrations version-controlled.
- After major stages: check structure, TS errors, lint, relevant tests, fix, update docs.
- Maintain **`PROJECT_STATUS.md`** (completed/in-progress/issues/pending, migrations, security status, latest tests). Do not mark complete if only UI exists.

---

## 13. Non-Functional Requirements

### 13.1 UX & Responsiveness
- Desktop/tablet/mobile (mobile-first). Loading/empty/error/offline states, upload progress, retry, keyboard nav, screen-reader labels, reduced motion. Consistent design system.

### 13.2 Performance
- Message pagination (never load full history), lazy loading, image optimization, video thumbnails, efficient queries, Redis caching, background jobs, media upload streaming, WS connection management, code splitting, efficient React rendering.

### 13.3 Testing (Automated)
- **Auth:** register/verify OTP (reject expired/incorrect), unique usernames, login/failures, password reset.
- **Private messaging:** A→B delivery, statuses, receipts, encryption, unauthorized access denied.
- **Groups:** create/add/messaging/permissions, 25-member limit (reject 26th), removed members lose future access.
- **Media:** encrypted image/video/voice, unauthorized access denied.
- **Stories & streaks:** create/view/expire/privacy, streak calculation integrity.
- **Security:** unauthorized API, session protections, brute-force, access-control bypass, media auth, key exposure, OTP abuse. **No fake encryption in tests.**

---

## 14. Development Phases (Execution Order)

| Phase | Focus | Key Outcomes |
|---:|---|---|
| 1 | Project foundation | Inspect workspace, monorepo, frontend/backend/TS/Postgres/Prisma/Redis/Docker, docs, design system |
| 2 | UI/UX implementation | Auth screens, dashboard, chat/group/stories/profile/settings, magical transitions |
| 3 | Authentication backend | Registration, OTP, username validation, login, password/session mgmt, security controls |
| 4 | Private messaging | Conversations, realtime, persistence, receipts, typing, reactions, edit/delete |
| 5 | Group messaging | Creation, members/roles, group encryption, realtime, enforce 25-member cap |
| 6 | Media & voice | Recording/playback, photo/video upload, encrypted storage, previews |
| 7 | Social features | Stories/viewers/reactions, disappearing messages, streaks, notifications |
| 8 | Security | Complete E2EE, device key mgmt, secure media encryption, security/access-control testing, threat-model review |
| 9 | Quality assurance | Automated tests, bug fixes, responsive/perf checks, migrations/realtime verification, security review |
| 10 | Deployment | Prod env/DB/Redis/email/storage/HTTPS/monitoring/backup, deploy, document setup |

---

## 15. Non-Negotiable Rules

1. No visual-only prototype / fake backend / hardcoded messages as system / simulated delivery.
2. No local state replacing persistent DB storage.
3. No homemade encryption; no exposed private keys; no plaintext passwords; no publicly accessible private media.
4. No dead important buttons; no unnecessary/out-of-scope features (no calls, no video calls, no camera filters).
5. No accessibility sacrificed for animations.
6. No production-readiness claims without tests and security verification.
7. Document required env vars for third-party services and explain developer configuration.

---

## 16. Final Delivery Checklist

- Complete source (frontend, backend, organized structure) + PostgreSQL schema & migrations.
- Working auth/email verification, realtime private & group messaging (25 max), voice/photo/video, stories/streaks, disappearing messages.
- Real E2EE implementation, responsive magical UI/UX, automated tests, Docker config, `.env.example`.
- Docs: README (install/env/DB/Redis/migrations/email/start/run tests/build/deploy), setup & deployment instructions, security docs, project status report.

---

## 17. Immediate Next Actions (from master prompt intent)

1. **Inspect current workspace** (`D:\ananta`) and existing files.
2. **Define and create architecture/project structure** (monorepo scaffold as above).
3. **Implement in phased order** (foundation → UI → auth → messaging → groups → media → social → security → QA → deployment).
4. **Maintain `PROJECT_STATUS.md` and docs** continuously; test before marking features complete.
5. **Prioritize:** correctness, security, maintainability, and extraordinary UX.
