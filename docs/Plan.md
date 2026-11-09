# #06 Plan: XPTNOW Escrow Marketplace

The design plan: how the work has gone so far, what comes next, and the exact prompt used to build the clickable demo, so anyone can rebuild or extend it.

---

## 1. Decision log

| Date (2026) | Decision |
|---|---|
| 7 Nov | Brief = two investor slides (Service Bonds, Online Deposits). Goal: a clickable **investor demo**, built as a Claude Artifact and kept separate from the xptnow.ai codebase. |
| 7 Nov | v1 to v6 explored a **one-page** demo (no sidebar), with tabs and timelines. |
| 7 Nov | Theme fixed to **white and navy blue**, with no dark mode. |
| 7 Nov | One-page rule withdrawn. New direction: **one login portal with role-based consoles (RBAC)** and a collapsible, organized sidebar. v7 published (Customer, Contractor, Developer / Reviewer bond portal). |
| 7 Nov | The manager asked for deposits in **XPT points, not MYR**. Decision: **no RM anywhere**. "Online Deposits" renamed **"XPT Hold"**, because buyers should see their points as on hold, not paid. |
| 9 Nov | **Plan change: Service Provider** = anyone who sells a service (contractor, shop owner, retailer, dealer, rental, freelancer). Providers **register and pick a profession first**, have an **XPT wallet** and **set their own XPT prices**. The **Developer role is dropped**; the **Reviewer** takes its work and has most priority tasks. |
| 9 Nov | **Escrow model** (like freelancer.com): the provider quotes the customer by XPT ID → the customer approves → points leave the customer's wallet and are **held by the Reviewer (XPTNOW, the points owner)** → provider uploads POD → customer requests release → Reviewer releases. |
| 9 Nov | The provider can **file a claim 3 days after POD** if the customer never replies. Release is **never automatic, only by hand by a Reviewer**. |
| 9 Nov | v8 published (escrow marketplace) and approved by the designer. These docs updated to match. XPT Hold is now covered by the escrow flow. |

## 2. Phases

| Phase | What | Owner | Status |
|---|---|---|---|
| 1. Discovery | Read the slides, agree the roles, flows and scope | Design | ✅ Done |
| 2. Clickable demo | Login + registration + 3 consoles, RBAC, escrow flow, timeline, notifications | Design (with Claude) | ✅ v8 live |
| 3. Design docs | PRD, AppFlow, UI_Spec, TRD, Schema, Plan | Design | ✅ Updated for v8 |
| 4. Review | Manager, investor and programmer feedback on the demo and docs | Manager, programmer | ⏳ Next |
| 5. Brand pass | Swap in the official SVG logo and exact brand colours | Design | ⏳ Waiting for assets |
| 6. Points economy | How users get XPT, expiry, fees | Product owner | ⏳ Open questions |
| 7. Build | Production app per the [TRD](TRD.md) | Engineering | Not started |

## 3. Suggested enhancements (backlog)

1. Provider **reply to a concern** shown on the dispute screen. ✅ (in v8)
2. **Customer ratings** and verified badges in Find a provider. ✅ (in v8)
3. In-app **chat** between customer and provider on a job.
4. **Milestone payments** for big jobs (e.g. a renovation split into 3 holds).
5. Upload real photos in the demo (file picker) instead of placeholder tiles.
6. A presenter-notes toggle, with one line to say per screen during an investor demo.
7. "?" help tips on XPT, escrow, POD and claims for non-technical viewers.
8. A Bahasa Malaysia language toggle.
9. A Reviewer SLA timer on release requests (e.g. "Release within 24 hours").

## 4. Open questions

- How customers and providers get XPT points (top-up, rewards, purchase).
- Do XPT points expire?
- Does XPTNOW take a fee when points are released?
- Is there a time limit for a Reviewer to act on a release request?
- Where XPT lives technically (own ledger, partner wallet or chain). See [TRD §4](TRD.md#4-xpt-escrow-and-ledger-requirements).
- Official brand assets.

## 5. How to view the demo

- **Live artifact:** [XPTNOW Escrow Demo](https://claude.ai/artifact/K73aknsXmo8UgSJqHWmBGE)
- **In this repo:** open [`demo/index.html`](../demo/index.html) in any browser. It needs no install or build.
- **Demo path (about 3 minutes):** sign in as **Service Provider** → Send a quote → Use the demo customer → Send quote → *(user menu → Switch role)* **Customer** → Quotes → Approve and hold 500 XPT → **Service Provider** → Jobs → add both POD items → Upload POD and mark done → **Customer** → My services → I'm satisfied, request release → **Reviewer** → Release requests → Release.
  - Claim path: after POD, as the provider, press "Skip 1 day (demo)" three times → File a claim → **Reviewer** → Claims.
  - Dispute path: as the customer, Raise a concern → **Reviewer** → Disputes → Apply split.
  - Registration: sign out → Service Provider → Register your business → pick a profession → Submit → **Reviewer** → Provider approvals → Approve.

---

## 6. Build prompt (exact)

This is the consolidated prompt that reproduces the v8 demo. Paste it into Claude (or another capable model) to rebuild it as one HTML file. It brings together everything that was asked for and agreed during the design sessions.

````text
Build a clickable investor demo for "XPTNOW Escrow Marketplace" as ONE self-contained HTML file
(HTML + CSS + vanilla JavaScript, no framework, no build step). Fonts: Inter (UI) and
JetBrains Mono (numbers, IDs, times) from Google Fonts. Icons: small inline SVGs.
It is a showcase only: all data is mocked in memory, nothing is sent anywhere.

CONTEXT
An escrow marketplace like freelancer.com, paid in XPT points. Example: a mobile shop
(service provider) repairs a phone for 500 XPT. The provider sets the price and sends a quote
to the customer by the customer's XPT ID. When the customer approves, 500 XPT leaves the
customer's wallet and is HELD BY XPTNOW (the Reviewer, who owns the points). The provider does
the work and uploads POD (proof of delivery). A satisfied customer asks the Reviewer to release
the points to the provider. If the customer never replies, the provider can file a claim
3 DAYS AFTER POD. Release is NEVER automatic: a Reviewer always releases by hand.
Use XPT points ONLY. Never show RM or any other currency. Say "on hold", not "deposit".

THEME (single light theme, no dark mode): white and navy blue only.
Tokens: bg #F5F7FB, surface #FFFFFF, surface-2 #F1F4FA, ink #0B1735, muted #5A6580,
line #E1E6EF, brand #1B3A8C, brand-hover #2A4FB0, brand-soft #E8EDF8, navy #0F1E45,
navy-2 #162A5C, ok #0F7A47/#E2F4EA, warn #9A5B00/#FFF1D9, bad #B4302A/#FDE6E4, badge #E2A23B.
Logo: text wordmark "XPTNOW", weight 900, "XPT" in brand blue (light blue on navy).
Card radius 14px, buttons 10px. Very user friendly, plain language.

ROLES (RBAC, 3 roles)
- Customer: Aina Rahman, XPT ID XPT-C-10482, wallet 2,000 XPT.
- Service Provider: FixIt Mobile, XPT-P-2201, profession "Mobile & electronics repair",
  verified, 4.8 stars, wallet 3,200 XPT. Catalog: Phone screen repair 500, Battery
  replacement 250, Water damage check 150.
- Reviewer: Nur Aisyah, XPTNOW (neutral escrow holder; does all priority work).
PROFESSIONS (with XPT price limits per service): Contractor 100-10,000; Mobile & electronics
repair 50-3,000; Shop owner / Retailer 20-5,000; Dealer 100-20,000; Rental 50-5,000;
Freelancer / Other 20-5,000.

STRUCTURE
1) LOGIN PORTAL (one login for everyone). Left navy panel: logo, headline "Pay for services
   with XPT points, safely held until the job is done.", 3 numbered steps. Right: "Sign in",
   3 role cards (Customer, Service Provider, Reviewer), email pre-filled per role, any password
   works, button "Sign in as <Role>". When Service Provider is selected show
   "New provider? Register your business".
2) PROVIDER REGISTRATION (3 steps with a step indicator): 1 "What do you do?" pick one of the
   6 profession cards; 2 "Your business": profession + price limit, business name, SSM number,
   location, email (pre-filled "Ah Seng Gadget Repair", Puchong) and "Submit for review";
   3 "Application sent", status pending review. It appears in the Reviewer's Provider approvals.
3) APP SHELL. A navy sidebar, neatly organized into sections with expandable/collapsible
   headers; a collapse button shrinks it to icons only (tooltips, badges on icons); on phones
   it is an off-canvas drawer with a hamburger and scrim. Clicking an item changes the view.
   Amber badges show items that need the user. Top bar: portal name + page title, "Investor
   demo" pill, notifications bell with unread count and pop-over, and a user menu with
   "Switch role (demo)" (3 roles), "Restart demo" and "Sign out". Toasts confirm every action.
   Menus:
   - Customer: [My account] Home, Find a provider, Quotes, My services; [Money] XPT wallet;
     [Support] Help & FAQ
   - Service Provider: [Overview] Dashboard; [Work] My services, Send a quote, Jobs;
     [Money] XPT wallet; [Account] Business profile
   - Reviewer: [Overview] Priority queue; [Escrow] Release requests, Claims, Disputes,
     Escrow ledger; [Providers] Provider approvals, All providers; [Admin] All jobs,
     Roles & permissions
4) ONE LIVE JOB ESC-3120 flows through all roles (Phone screen repair, 500 XPT, complete by
   12 Nov 2026). States: none -> quoted -> held -> pod -> requested | claimed | disputed ->
   released | refunded | split; quoted -> declined; claimed -> pod when a claim is rejected.
   - Provider "Send a quote": customer XPT ID field + "Find" (or "Use the demo customer"),
     shows "Aina Rahman found"; service select (fills the catalog price), amount (validated
     against profession limits), description, complete-by date, "Send quote".
   - Customer "Quotes": quote card, escrow explainer, "Approve and hold 500 XPT"
     (disabled if not enough points) or "Decline". Approve moves 500 from available to on hold.
   - Provider "Jobs": job details, navy escrow box "500 XPT held safely by XPTNOW", two POD
     tiles (Repaired phone photo, Handover note signed), "Upload POD and mark done" enabled
     only when both are added. Then a 3-day countdown (3 bars) with "Skip 1 day (demo)" and
     "File a claim" enabled on day 3. If the customer raised a concern, a reply box.
   - Customer "My services": POD tiles (read only) and "Are you happy with the service?":
     "I'm satisfied, request release" or "Raise a concern" (reason select: Screen still
     flickers / Touch not working in one corner / Phone returned with scratches).
   - Reviewer: Release requests -> "Release 500 XPT to FixIt Mobile"; Claims -> "Approve claim, release 500 XPT" or
     "Reject claim" (back to POD, customer reminded); Disputes -> three side cards (customer
     says, provider says, evidence), refund input (default half), "Apply split",
     "Release all to provider", "Refund all to customer".
   - Closing updates both wallets and their transactions, shows the outcome (green/amber split
     bar for a split), and lets the customer rate 1-5 stars.
   NEVER auto-switch roles or views after an action; only the user switches.
5) JOB TIMELINE on every work screen (right-hand card): vertical steps with time and actor;
   done = filled navy dot with a check, current = ringed dot + "NOW", upcoming = hollow; the
   viewer's own steps carry a "YOU" tag. Every work screen also shows a "Waiting on: <role>"
   box that starts with "Your turn." for the owner.
6) EXTRA SCREENS: Customer Home (navy greeting with XPT ID pill + Copy, stat tiles available /
   on hold / open services / completed, "Needs your attention", flow line of the 4 steps).
   Find a provider (profession filter chips, provider cards with initials logo, verified badge,
   rating, jobs, "from" price, "Ask for a quote"; sample: FixIt Mobile, CoolAir Services
   (contractor), LensHire KL (rental), Kayu Craft Studio (retail)). XPT wallets with
   transactions. Help & FAQ accordions (What are XPT points? What does "on hold" mean? What is POD?
   What if I am not happy? What if I forget to reply?).
   Provider Dashboard (stats + "Needs your action"), My services (editable prices with XPT
   suffix and limit check), Business profile. Reviewer Priority queue (stat tiles: points in
   escrow, waiting on you, avg time to release 1.6 days, dispute rate 6%, labelled sample
   pilot data; queue items with "Open"), Escrow ledger, Provider approvals (pending Kilat
   Rentals + any new registration; Approve / Reject), All providers, All jobs table with
   filters and search (sample ESC-3117, ESC-3109 held; ESC-3098, ESC-3085 released),
   Roles & permissions matrix (rows = permissions, columns = 3 roles, current role highlighted).
7) NOTIFICATIONS: each hand-off notifies the next role(s) (see the AppFlow doc).
8) QUALITY: responsive down to 360px with no sideways page scroll (tables scroll inside their
   own container), visible focus states, Escape closes pop-overs, respect
   prefers-reduced-motion, empty states that explain what will appear. Login note: "Demo only. Any
   password works, and nothing leaves this page."
````
