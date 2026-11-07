# #04 TRD: XPTNOW Service Bond Portal

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
| Database | **PostgreSQL** | Relational data, transactions for bond and ledger moves |
| Auth | Managed auth (e.g. Auth0, Clerk, or Supabase Auth) with email + OTP | Secure login without building it from scratch |
| Files | Object storage (S3-compatible) for photos | Evidence photos and ponding-test images |
| Notifications | In-app first; email/WhatsApp later | Matches the bell and notification events in the demo |
| AI evidence check | A vision model call that compares uploaded photos against the acceptance criteria | Powers "AI can flag missing evidence" from the slides |

## 3. Access control (RBAC)

- Roles: `customer`, `contractor`, `developer`, `reviewer` (an `admin` role can be added later).
- **Every permission is checked on the server.** Hiding a menu item is not security.
- Data scoping:
  - A **Customer** sees only their own units and repairs.
  - A **Contractor** sees only work orders assigned to them, and their own wallet.
  - A **Developer** sees all work orders in their projects.
  - A **Reviewer** sees disputed jobs (plus read-only job lists) for the projects they are assigned to, and must not belong to the developer's organization.
- The permission matrix is in [Schema §3](Schema.md#3-rbac-permission-matrix).

## 4. XPT bond and ledger requirements

- The bond lock, release and deduction must be **atomic** (one database transaction, or the XPT ledger's own atomic call).
- Use a **double-entry ledger**: every move is two entries (e.g. contractor *available* → contractor *locked*). Balances are derived from the entries, never edited directly.
- The required amount must be locked in full before the job leaves `assigned`.
- Deduction ≤ locked amount. The released amount = locked − deduction.
- Idempotency keys on lock, release and decide actions, so a double-click can't double-move funds.
- **Open question:** whether XPT lives on xptnow.ai's own ledger, a partner wallet or a blockchain. The data model in [Schema](Schema.md) works with any of them, through `ledger_entries.external_ref`.

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
| Localisation | English first; plan for Bahasa Malaysia. Currency is RM, and dates use the en-GB format (5 Dec 2026). |
| Observability | Error tracking and metrics for the pilot measures (acceptance time, dispute rate, bond reuse) |

## 7. API sketch (illustrative)

| Method & path | Role | Action |
|---|---|---|
| `POST /reports` | customer | Submit a defect report |
| `POST /work-orders` | developer | Create a work order from a report |
| `POST /work-orders/:id/lock` | contractor | Accept and lock the bond |
| `POST /work-orders/:id/complete` | contractor | Upload proof and mark complete |
| `POST /work-orders/:id/inspect` | developer | Accept, or open a dispute |
| `POST /work-orders/:id/confirm` | customer | Confirm, or raise a concern |
| `POST /disputes/:id/reply` | contractor | Add the contractor's side |
| `POST /disputes/:id/decide` | reviewer | Approve a deduction or reject |
| `GET /work-orders/:id/timeline` | all (scoped) | Timeline events |
| `GET /wallet` | contractor | Balances and transactions |
| `GET /notifications` | all | The user's notifications |
