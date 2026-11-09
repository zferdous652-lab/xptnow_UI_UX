# XPTNOW UI/UX: Escrow Marketplace

The UI/UX design handoff for the **XPTNOW Escrow Marketplace**: a portal where customers pay service providers in **XPT points**, the points are **held by XPTNOW** while the work is done, and an XPTNOW reviewer **releases them by hand** once the work is proven.

> **Design phase only.** This repo holds the design docs and a clickable demo. It is not production code and is not connected to xptnow.ai.

## See it

- **Live demo:** [XPTNOW Escrow Demo](https://claude.ai/artifact/K73aknsXmo8UgSJqHWmBGE)
- **Offline:** download [`demo/index.html`](demo/index.html) and open it in any browser. No install needed.

**3-minute demo path:** sign in as **Service Provider** → Send a quote → Use the demo customer → Send quote → *(user menu → Switch role)* **Customer** → Quotes → Approve and hold 500 XPT → **Service Provider** → Jobs → add both POD items → Upload POD → **Customer** → My services → *I'm satisfied, request release* → **Reviewer** → Release requests → Release.
Also try: *Skip 1 day* ×3 → File a claim (claim path), *Raise a concern* → Reviewer → Disputes → Apply split (dispute path), and *Register your business* on the sign-in page (provider registration).

## How it works

1. A **Service Provider** (contractor, repair shop, retailer, dealer, rental, freelancer) sends a quote in XPT points to a customer's XPT ID.
2. The **Customer** approves. The points leave their wallet and are **held by XPTNOW**.
3. The provider does the work and uploads **proof of delivery (POD)**.
4. The customer requests release, and the **Reviewer** releases the points. If the customer doesn't reply within **3 days of POD**, the provider can file a claim.

Release is never automatic. Amounts are **XPT points only** (no RM).

## Who uses it (RBAC)

| Role | Console | Can do |
|---|---|---|
| Customer | Customer portal | Find providers, approve quotes, request release or raise a concern, rate |
| Service Provider | Service Provider portal | Register with a profession, set XPT prices, send quotes, upload POD, file claims |
| Reviewer (XPTNOW) | Reviewer portal | Hold points, release or refund by hand, decide claims and disputes, approve providers |

## Docs (`/docs`)

| # | Doc | For | What's inside |
|---|---|---|---|
| 01 | [PRD.md](docs/PRD.md) | Manager, investor | Problem, users, scope, requirements, business rules, the source investor slides |
| 02 | [AppFlow.md](docs/AppFlow.md) | Everyone | Login and registration, escrow state machine, happy, claim and dispute flows (diagrams) |
| 03 | [UI_Spec.md](docs/UI_Spec.md) | Designer, programmer | Colours, type, layout, components, screens, copy |
| 04 | [TRD.md](docs/TRD.md) | Programmer | Recommended stack, RBAC, XPT escrow and ledger rules, non-functional requirements, API sketch |
| 05 | [Schema.md](docs/Schema.md) | Programmer | Data model (ER diagram), tables, permission matrix, professions and seed data |
| 06 | [Plan.md](docs/Plan.md) | Manager | Decisions, phases, backlog, open questions, **the exact prompt used to build the demo** |

## Feedback

Please leave comments on the docs through a pull request or an issue. Brand colours and logo are placeholders until the official assets arrive. Open questions (how users get XPT points, expiry, fees) are listed in the [Plan](docs/Plan.md#4-open-questions).
