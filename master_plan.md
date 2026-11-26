# Ikirezi — Master Plan & Architecture

> **Ikirezi** (Kinyarwanda: *a precious pearl*) — an SRH education, wellbeing and opportunity platform for girls and young women, built by TeleMedAfrica.
> Tagline: **Know your body. Own your future.** · Kinyarwanda: *Menya umubiri wawe. Tegura ejo hawe.*

Version 1.0 · 4 November 2026 · Status: Approved for build

---

## 1. Problem statement

Girls and young women in Rwanda and the wider East African region are reaching puberty, relationships and adulthood without reliable, private, judgement-free information or a safe route to a qualified professional.

| Indicator | Figure | Source |
|---|---|---|
| Girls aged 15–19 in Rwanda who have ever been pregnant | **8%** in the 2025 survey, up from ~5% in 2019–20 | The New Times, Mar 2026 |
| Pregnancies among 15–19-year-olds in low and middle income countries each year | **21 million**, ~50% unintended, ~12 million births | WHO / UNFPA |
| Adolescent birth rate, sub-Saharan Africa (highest region) | **97.9 per 1,000** girls aged 15–19 (2023) | WHO |
| Births to girls aged 10–14 in sub-Saharan Africa | **332,000** (2021) | WHO |
| Unintended adolescent pregnancies ending in abortion (often unsafe) | **55%** | UNFPA |
| Girls in sub-Saharan Africa who miss school during their period | **~1 in 10** | UNESCO via World Bank |
| Kenyan girls' learning days lost to menstruation over 4 years of high school | **~165 days** | World Bank blog |
| Rwanda unemployment rate, women vs men (Q3 2024) | **18.5% vs 12.5%** · youth 18.8% | NISR LFS Q3 2024 |
| Rwanda employment-to-population ratio, women vs men | **45.4% vs 64.3%** | NISR LFS Q3 2024 |
| Rwanda median age | **19.9 years** | DataReportal 2026 |
| Rwanda mobile connections | **13.0M (88.7% of population)**; internet users 5.0M (34.2%) | DataReportal 2026 |

**Core problems**
1. **Information gap** — SRH content is scattered, often in English only, and mixed with myths on social media.
2. **Stigma & privacy** — girls fear being identified when they ask about periods, contraception, STIs or relationships.
3. **Access to professionals** — few youth-friendly counsellors, no fair queue, no follow-up.
4. **Period poverty & hygiene literacy** — missed school days, no tracking, little nutrition guidance.
5. **Opportunity gap** — young women are less employed and rarely see scholarships, programmes and low-cost ideas aimed at them.
6. **Unsafe AI** — general chatbots hallucinate medical facts and cannot cite sources.

## 2. Proposed solution

A mobile-first platform (Flutter app) with an admin dashboard and a professionals portal (Next.js), backed by a secure FastAPI service and **source-grounded AI**:

- **Learn** — reviewed SRH library (lessons, FAQs, quizzes) in English and Kinyarwanda.
- **Share** — anonymous stories (AI-screened, admin-approved) and named posts by vetted contributors on a weekly topic.
- **Care** — cycle & hygiene tracker, cited hygiene and nutrition tips, nutrition and SRH chatbots that answer only from a curated corpus.
- **Grow** — opportunities feed (academic, jobs, life skills, scholarships, zero-cost ideas), "Mark my calendar" reminders, weekly challenges, women's-day celebrations, "Today in History: great women".
- **Get help** — request a professional (2 users per professional per day, first-come-first-served), referral tracking, emergency contacts.

## 3. Expected impact (targets, 24 months)

| Outcome | Baseline evidence | Target metric |
|---|---|---|
| SRH knowledge | Only a minority of adolescent girls have comprehensive HIV knowledge (Rwanda DHS 2019-20) | +30% average quiz score pre→post per learner |
| Safe referrals | No structured youth referral path | 10,000 professional sessions served, median wait < 3 days |
| School days protected | ~1 in 10 girls miss school during periods | 60% of active trackers report "no missed days" in month 3 |
| Opportunity reach | Women unemployment 18.5% | 25,000 opportunity saves; 5,000 applications self-reported |
| Safe AI | General LLMs hallucinate | 100% of AI answers carry ≥1 citation or a refusal; 0 diagnoses |
| Reach | 13M mobile connections | 100,000 registered users, 35% monthly active |

Impact model: knowledge (library, AI) → confident decisions → earlier care-seeking (professional queue) → fewer unintended pregnancies & missed school days → better education and economic outcomes (opportunities).

## 4. Users & roles

| Role | Surface | Abilities |
|---|---|---|
| Guest | Mobile | Onboarding, browse public library previews, emergency contacts, language toggle |
| Member (girl/young woman) | Mobile | Everything below after sign-up + email OTP |
| Contributor (privileged poster) | Mobile | Member + publish named posts on the weekly topic (AI-reviewed every time) |
| Professional (doctor/counsellor) | Pro portal | See assigned members (alias only), manage sessions, write posts, answer routed questions |
| Content reviewer | Admin dashboard | Review library items, AI-curated insights, stories |
| Admin / Super admin | Admin dashboard | Full management, analytics, audit logs, security |

## 5. Feature map

### 5.1 Mobile app (Flutter, MVVM + Provider)
| # | Module | Key features | Gate |
|---|---|---|---|
| M1 | Onboarding | 4 illustrated slides, language picker (EN/RW), privacy promise | Public |
| M2 | Auth | Sign up (alias, email, password, birth year, country, consent), login = password **+ email OTP**, forgot password, device binding | Public |
| M3 | Home | Greeting, celebration banner, Today in History, weekly topic, challenge, quick actions | Public preview / full for members |
| M4 | Learn | Library by topic, lesson reader with citations, quizzes, bookmarks | Previews public, full = member |
| M5 | Stories | Feed of anonymised stories, like + 1–5 stars, submit story (AI → admin) | Read = member |
| M6 | Posts | Contributor posts on weekly topic, like + stars, compose (contributors only) | Member |
| M7 | Ask a question | Suggest topics/questions → admin routes to contributors/professionals | Member |
| M8 | Care: Cycle | Log period days, flow, symptoms, mood; predictions; calendar | Member |
| M9 | Care: Hygiene | Daily hygiene checklist, streaks, cited hygiene + nutrition tips (tap to open source) | Member |
| M10 | AI chat | Two assistants: **SRH Guide** and **Nutrition Coach**, grounded answers with citations, refusal + referral when ungrounded | Member |
| M11 | Opportunities | Filters (academic, jobs, life skills, scholarships, competitions, zero-cost ideas), detail, save, **Mark my calendar** with notification | Member |
| M12 | Grow | Weekly challenges, badges, "Be an opportunity to others" ideas | Member |
| M13 | Professionals | Request professional → FIFO queue → assigned professional & date revealed; history; cancel | Member |
| M14 | Emergency | One-tap call list (112, 912, 3512, 116, 114, 3511) + saved trusted contact; works offline | Public |
| M15 | Profile & Settings | Avatar picker, alias, language, notifications, reminder times, discreet mode (app lock / PIN), data export, delete account, logout (confirm) | Member |
| M16 | Notifications | Reminders, story status, booking updates, strikes | Member |

### 5.2 Admin dashboard (Next.js)
Overview KPIs · Users · Stories moderation · Posts · Contributors · Professionals · Bookings queue · Suggested questions (route) · AI Curator queue (insights, history, opportunities drafts) · Opportunities · Library · Weekly topics · Challenges · Celebrations · Emergency contacts · Analytics · Audit log (history) · Security (MFA, sessions) · Settings · Profile · Logout with confirmation.

Every list has search, status filter, date range filter, country filter and CSV export; every mutation writes an audit-log entry.

### 5.3 Professionals portal (Next.js)
Login + TOTP MFA · Today (assigned members, max 2/day) · Schedule & history · Session notes (encrypted) & referral status · Routed questions · My posts (compose → AI review status) · Profile & availability · Logout with confirmation.

## 6. Key business rules

1. **Accounts** — one account per email; phone & device fingerprint hashes are stored; banned identifiers are added to a hashed blocklist so they cannot register again.
2. **Login** — password then 6-digit email OTP (5 min TTL, 5 attempts, rate-limited). Admin & professionals additionally require TOTP MFA.
3. **Stories** — any member can submit; AI moderation (safety, PII scrub, medical accuracy against corpus) → `ai_approved | ai_rejected`; admin approves → `published`. Display never shows alias, avatar or any identifier.
4. **Posts** — only contributors (added by admin and meeting qualification) can post; every post is AI-reviewed; must belong to the **active weekly topic** (topic lasts 7 days).
5. **Strikes** — each AI rejection for abusive language adds a strike; the user is warned on strikes 1–4; at **5 strikes** posting rights are permanently revoked and the user is notified.
6. **Professional booking** — capacity **2 members per professional per day**; requests are queued FIFO by `created_at`; the allocator assigns the earliest day with a free slot across active professionals until everyone is served. Professional details are revealed only after the member taps *Request professional*.
7. **AI never hallucinates** — retrieval-augmented generation over an allow-listed corpus (WHO, UNFPA, UNICEF, CDC, NHS, Rwanda MoH/RBC); answers must cite retrieved passages; if retrieval confidence is below threshold the assistant refuses and offers a professional. No diagnosis, no pregnancy confirmation.
8. **AI Curator** — scheduled job pulls fun facts, opportunities and "Today in History" women per country from allow-listed sources into a draft queue; nothing is published without admin approval.
9. **Suggested questions** — members suggest → admin assigns to a contributor/professional → answer post links back to the question.

## 7. Architecture

```
┌────────────────┐   ┌──────────────────┐   ┌──────────────────┐
│ Flutter app    │   │ Admin (Next.js)  │   │ Pro portal (Next)│
│ MVVM+Provider  │   │ App Router, RSC  │   │ App Router       │
└──────┬─────────┘   └────────┬─────────┘   └────────┬─────────┘
       │ HTTPS/JSON, JWT       │                      │
       └───────────────┬───────┴──────────────────────┘
                ┌──────▼───────────────────────────┐
                │ FastAPI  /api/v1                 │
                │ routers → services → repositories│
                │ schemas (Pydantic) · models (ORM)│
                │ auth: JWT, OTP, TOTP, RBAC deps  │
                │ rate limit · audit middleware    │
                └──┬────────────┬──────────┬───────┘
          ┌────────▼───┐  ┌─────▼─────┐ ┌──▼──────────────┐
          │ PostgreSQL │  │ Redis     │ │ AI provider     │
          │ (pgvector) │  │ OTP, RL,  │ │ LLM + embeddings│
          └────────────┘  │ queues    │ └─────────────────┘
                          └───────────┘
   Email (SMTP/SES) · Push (FCM) · Object storage (S3) · Scheduler (APScheduler)
```

### 7.1 Backend folder layout (`backend/app`)
```
core/         config, database, security (hashing, JWT, TOTP), dependencies (RBAC)
models/       SQLAlchemy ORM entities
schemas/      Pydantic request/response models
repositories/ data access only (one per aggregate)
services/     business rules (auth, otp, moderation, ai, booking, curator, ...)
routers/      HTTP endpoints, thin; each endpoint declares its role dependency
main.py       app factory, middleware, router registration
```

### 7.2 Mobile layout (`mobile/lib`)
```
core/        theme, constants, router, l10n strings (EN/RW), api client, storage
models/      plain Dart data classes (fromJson/toJson)
services/    API services (auth, content, stories, cycle, chat, booking...)
viewmodels/  ChangeNotifier classes (one per screen/feature)
views/       screens grouped by feature + shared widgets
```

### 7.3 Data model (core entities)
`User(id, alias, email, password_hash, role, avatar, birth_year, country, language, email_verified, mfa_secret, can_post, strikes, posting_banned, is_active, created_at)` · `DeviceFingerprint` · `BlockedIdentifier(hash, kind)` · `OtpCode` · `RefreshToken` · `Story(status, body, topic_id, ai_verdict, ai_reasons, likes, rating_sum, rating_count)` · `Post(author_id, topic_id, status, ai_verdict)` · `Reaction(user, target, kind, stars)` · `WeeklyTopic(title, starts_on, ends_on)` · `Lesson / Quiz / QuizAttempt` · `CycleLog / HygieneLog` · `Tip(kind, body, source_title, source_url)` · `Opportunity(category, deadline, cost_level, url)` · `CalendarReminder` · `Professional(user_id, specialty, daily_capacity=2)` · `BookingRequest(status, assigned_professional_id, scheduled_for, queue_position)` · `SuggestedQuestion(status, assignee_id)` · `CuratedItem(kind, payload, sources, status)` · `Challenge / ChallengeCompletion` · `Celebration(date, title)` · `EmergencyContact(country, label, number)` · `Notification` · `AuditLog(actor_id, action, entity, entity_id, meta, ip, created_at)` · `ChatMessage(assistant, question, answer, citations)`.

## 8. API surface (v1) & security

All routes are under `/api/v1`. Every endpoint declares one of: `public`, `member`, `contributor`, `professional`, `reviewer`, `admin`. Additional protections: HTTPS only, JWT (15 min access / 7 day rotating refresh), bcrypt, rate limits on auth and AI, CORS allow-list, request-size limits, input validation, audit logging, PII scrub before AI calls, field-level encryption for session notes.

| Area | Endpoints | Guard |
|---|---|---|
| Auth | `POST /auth/register`, `/auth/login`, `/auth/verify-otp`, `/auth/resend-otp`, `/auth/refresh`, `/auth/logout`, `/auth/mfa/setup`, `/auth/mfa/verify`, `/auth/forgot-password`, `/auth/reset-password` | public / member |
| Me | `GET/PATCH /me`, `GET /me/history`, `GET /me/export`, `DELETE /me` | member |
| Library | `GET /lessons`, `GET /lessons/{id}`, `POST /lessons/{id}/quiz`, admin CRUD | public preview / member / reviewer |
| Stories | `GET /stories`, `POST /stories`, `POST /stories/{id}/react`, `GET /admin/stories`, `POST /admin/stories/{id}/decision` | member / admin |
| Posts | `GET /posts`, `POST /posts`, `POST /posts/{id}/react` | member / contributor |
| Topics | `GET /topics/active`, admin CRUD | public / admin |
| Questions | `POST /questions`, `GET /questions/mine`, `GET /admin/questions`, `POST /admin/questions/{id}/assign` | member / admin |
| Care | `GET/POST /cycle/logs`, `GET /cycle/prediction`, `GET/POST /hygiene/logs`, `GET /tips` | member |
| AI | `POST /ai/chat` (`assistant=srh|nutrition`) | member, rate-limited |
| Opportunities | `GET /opportunities`, `POST /opportunities/{id}/remind`, admin CRUD | member / admin |
| Booking | `POST /bookings`, `GET /bookings/mine`, `DELETE /bookings/{id}`, `GET /pro/bookings`, `PATCH /pro/bookings/{id}` | member / professional |
| Engagement | `GET /challenges/current`, `POST /challenges/{id}/complete`, `GET /celebrations/today`, `GET /history/today` | public / member |
| Emergency | `GET /emergency?country=RW` | public |
| Admin | `/admin/metrics`, `/admin/users`, `/admin/contributors`, `/admin/professionals`, `/admin/curated`, `/admin/audit`, `/admin/settings` | admin |

## 9. Design system summary
- **Palette** — Ikirezi Plum `#4A1D5E` (primary), Orchid `#7B3F98`, Sunrise Coral `#F26D5B` (action), Pearl Blush `#FFF6F2` (background), Care Teal `#1F8A82` (health/success), Star Gold `#F5B83D` (ratings/celebration), Ink `#1E1726`, Mist `#F3EEF7`, Alert Red `#D93F4C`.
- **Type** — *Fraunces* (display, warm & editorial) + *Plus Jakarta Sans* (UI & body).
- **Imagery** — illustrated avatars only, never real people's photos.
- **Tone** — warm, factual, never shaming; every health fact carries its source.

## 10. Roadmap
| Phase | Weeks | Scope |
|---|---|---|
| 0 Discovery | 1–2 | Content partners, corpus licensing, MoH/RBC alignment, ethics review |
| 1 MVP | 3–14 | Auth+OTP+MFA, onboarding, library, stories, posts, topics, cycle/hygiene, AI chat (RAG), opportunities + reminders, booking queue, emergency, admin core, pro portal core |
| 2 Growth | 15–24 | Analytics, challenges & badges, celebrations, Today in History, curator automation, offline mode, SMS OTP fallback |
| 3 Scale | 25+ | Voice AI in Kinyarwanda, USSD lite, more countries, partner integrations with TeleMedAfrica clinics |

## 11. Acceptance criteria (summary)
- Login always requires password **and** a valid email OTP; admin/pro additionally TOTP.
- A second account with a used email, phone or device fingerprint is rejected (`409`).
- No published story response contains `alias`, `email`, `user_id` or avatar.
- A post outside the active weekly topic is rejected (`422`).
- 5th abusive rejection sets `posting_banned=true`; later submissions return `403`.
- A professional never has more than 2 bookings on the same date; requests are served strictly by creation time.
- Every AI answer returns `citations[]` non-empty or `refused=true`.
- Every admin mutation creates an `AuditLog` row; audit log is filterable by actor, action, date.
- Logout on all web portals opens a confirmation dialog first.
- All endpoints reject missing/expired tokens with `401` and wrong role with `403` (covered by tests).

## 12. Non-functional requirements
Availability 99.5% · p95 API < 400 ms (non-AI) · App cold start < 3 s on 2 GB RAM Android · Works on 3G; images < 150 KB · WCAG 2.1 AA contrast · Data residency in-region where possible · Compliant with Rwanda Law N° 058/2021 on personal data protection.

## 13. Risks & mitigations
| Risk | Mitigation |
|---|---|
| Harmful or false content | Double gate (AI + human), allow-listed sources, reporting |
| Minor safety | No DMs between members; professionals see alias only; safeguarding escalation path |
| Professional capacity | FIFO queue transparency, waitlist ETA, partner clinics |
| Cost of AI | Caching, small models for moderation, per-user rate limits |
| Low connectivity | Offline emergency list, cached lessons, lightweight assets |

## Sources
- The New Times — Teen pregnancy rises to 8%: https://www.newtimes.co.rw/article/34149/news/health/teen-pregnancy-rises-to-8-as-experts-warn-of-growing-risks
- WHO — Adolescent pregnancy fact sheet: https://www.who.int/en/news-room/fact-sheets/detail/adolescent-pregnancy
- UNFPA — Adolescent pregnancy: https://www.unfpa.org/adolescent-pregnancy
- World Bank — Menstrual health and hygiene: what role can schools play?: https://blogs.worldbank.org/en/education/menstrual-health-and-hygiene-what-role-can-schools-play
- NISR — Labour Force Survey Q3 2024 key findings: https://statistics.gov.rw/publication/key-findings-labour-force-survey-%E2%80%93-quarter-3-2024
- DataReportal — Digital 2026: Rwanda: https://datareportal.com/reports/digital-2026-rwanda
- Republic of Rwanda — Emergency numbers: https://www.gov.rw/emergency
- HSRC — mHealth & adolescent SRH scoping review: https://hsrc.ac.za/news/public-health/do-mhealth-interventions-enhance-adolescent-sexual-and-reproductive-health-service-uptake/

## 14. Deliverables map
| Package | Folder | Notes |
|---|---|---|
| Girls' app | `mobile/` | Flutter, MVVM + Provider |
| Admin console | `admin-dashboard/` | Next.js, port 3000, roles admin/super_admin |
| Professionals portal | `professional-portal/` | Next.js, port 3001, role professional, separate cookies (`ikp_*`) |
| API | `backend/` | FastAPI: models, repositories, schemas, services, routers |
| Brand | `docs/brand/` | logo SVGs, design tokens, style guide |
