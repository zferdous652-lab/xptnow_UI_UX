# #04 TRD: XPTNOW Escrow Marketplace

Technical requirements for turning the design into a real product. This is written by design for the engineering team. **The recommendations are suggestions, and the final technical decisions belong to the xptnow.ai engineers.**

---

## 1. Current state: the demo

| Item | Detail |
|---|---|
| Format | One self-contained HTML file (HTML + CSS + vanilla JavaScript), published as a Claude Artifact |
| Framework | None. Screens are functions that render HTML from one in-memory state object; clicks update the state and re-render. |
| Fonts | Inter and JetBrains Mono (Google Fonts) |
| Icons | Inline SVG, hand-drawn, 24px grid, 1.8 stroke |
| Data | Mocked in memory, so it resets on reload. The only thing stored is the sidebar collapse preference (`localStorage`). |
| Auth | None. Any password works, and roles are chosen on the login page. |

The demo is a **design spec you can click**, not production code.

## 2. Recommended production stack

| Layer | Suggestion | Why |
|---|---|---|
| Front end | **Next.js (React, TypeScript)** | Routing per console, server-side rendering, large ecosystem |
| UI kit | **Tailwind CSS + shadcn/ui** (or MUI) | Maps directly onto the tokens and components in [UI_Spec](UI_Spec.md) |
| Back end | Node.js (Next.js API routes or NestJS) | One language across the stack |
| Database | **PostgreSQL** | Relational data, transactions for escrow and ledger moves |
| Auth | Managed auth (e.g. Auth0, Clerk, or Supabase Auth) with email + OTP | Secure login without building it from scratch |
| Files | Object storage (S3-compatible) | POD photos, handover notes, business documents |
| Notifications | In-app first; email/WhatsApp later | Matches the bell and notification events in the demo |
| Scheduler | A job queue or cron (e.g. pg-boss, BullMQ) | Opens the claim window 3 days after POD and sends reminders. It never releases points. |
| Optional AI check | A vision model call that checks the POD photo against the quote | Helps the Reviewer spot missing proof faster |

## 3. Access control (RBAC)

- Roles: `customer`, `provider`, `reviewer` (an `admin` role can be added later).
- **Every permission is checked on the server.** Hiding a menu item is not security.
- Data scoping:
  - A **Customer** sees only their own quotes, jobs and wallet, plus the public list of verified providers.
  - A **Service Provider** sees only their own catalog, quotes, jobs and wallet. A customer is looked up by exact XPT ID only (no browsing of customers).
  - A **Reviewer** (XPTNOW staff) sees all jobs, the escrow ledger and provider applications, and must never decide a job they are part of.
- A provider account is `pending` until a Reviewer verifies it, and cannot send quotes until then.
- The permission matrix is in [Schema §3](Schema.md#3-rbac-permission-matrix).

## 4. XPT escrow and ledger requirements

- **XPT points only.** No fiat amounts are stored or shown in this product.
- Use a **double-entry ledger**. Every move is two entries, e.g. customer *available* → XPTNOW *escrow*, then XPTNOW *escrow* → provider *available*. Balances are derived from the entries, never edited directly.
- **Approve quote** = one atomic transaction: check the customer has enough available points, move the amount into escrow, and set the job to `held`.
- **Release, refund and split** = one atomic transaction each. Release + refund always equals the held amount.
- **No automatic release.** Only a request from a signed-in Reviewer can move points out of escrow. Scheduled jobs may open the claim window or send reminders, but never move points.
- A claim is allowed only when the job is in `pod`, at least **72 hours** after POD, with no release request or concern.
- Idempotency keys on approve, release, refund, split and claim, so a double-click can't double-move points.
- **Open questions:** where XPT lives (xptnow.ai's own ledger, a partner wallet or a chain), how users top up, whether points expire, and whether XPTNOW takes a fee. The data model in [Schema](Schema.md) works with any of these, through `ledger_entries.external_ref`.

## 5. Job state machine

- The states and allowed transitions are exactly those in [AppFlow §3](AppFlow.md#3-job-lifecycle-state-machine).
- Enforce transitions on the server: reject any action that is not valid from the current state, or not allowed for the caller's role.
- Write an **audit event** for every transition (who, when, from → to, payload). The job timeline is built from these events.

## 6. Non-functional requirements

| Area | Requirement |
|---|---|
| Performance | First screen under 2s on a mid-range phone over 4G |
| Responsive | 360px phones up to wide desktops; the page never scrolls sideways |
| Accessibility | WCAG 2.1 AA |
| Security | HTTPS only, server-side RBAC, signed upload URLs, rate-limited auth, audit log |
| Privacy | Customer personal data is visible only to roles that need it; follow Malaysia's PDPA |
| Reliability | Ledger and state changes are transactional; daily backups |
| Localisation | English first; plan for Bahasa Malaysia. Amounts are XPT points only, and dates use the en-GB format (12 Nov 2026). |
| Observability | Error tracking and metrics for the pilot measures (time to release, dispute rate, claim rate, provider approval time) |

## 7. API sketch (illustrative)

| Method & path | Role | Action |
|---|---|---|
| `POST /providers` | provider | Register a business with a profession (status `pending`) |
| `POST /providers/:id/decide` | reviewer | Approve or reject a registration |
| `GET /providers?profession=` | customer | Browse verified providers |
| `PUT /services/:id` | provider | Set a service's XPT price (within profession limits) |
| `GET /customers/lookup?xpt_id=` | provider | Check a customer XPT ID before quoting |
| `POST /quotes` | provider | Send a quote to a customer |
| `POST /quotes/:id/approve` | customer | Approve; moves points into escrow and creates the job |
| `POST /quotes/:id/decline` | customer | Decline |
| `POST /jobs/:id/pod` | provider | Upload POD (photo + handover note) |
| `POST /jobs/:id/request-release` | customer | Ask XPTNOW to release |
| `POST /jobs/:id/concern` | customer | Raise a concern (opens a dispute) |
| `POST /jobs/:id/claim` | provider | File a claim (72h+ after POD) |
| `POST /disputes/:id/reply` | provider | Add the provider's side |
| `POST /jobs/:id/decide` | reviewer | `release` · `refund` · `split {refund_xpt}` · `reject_claim` |
| `POST /jobs/:id/rating` | customer | Rate the provider after closing |
| `GET /jobs/:id/timeline` | all (scoped) | Timeline events |
| `GET /wallet` | customer, provider | Balances and transactions |
| `GET /escrow/ledger` | reviewer | All escrow movements |
| `GET /notifications` | all | The user's notifications |
