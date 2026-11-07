# #06 Plan: XPTNOW Service Bond Portal

The design plan: how the work has gone so far, what comes next, and the exact prompt used to build the clickable demo, so anyone can rebuild or extend it.

---

## 1. Decision log

| Date (2026) | Decision |
|---|---|
| 7 Nov | Brief = two investor slides (Service Bonds, Online Deposits). Goal: a clickable **investor demo**, built as a Claude Artifact and kept separate from the xptnow.ai codebase. |
| 7 Nov | v1 to v6 explored a **one-page** demo (no sidebar), with tabs for Contractor, Developer / Reviewer and Online Deposits, plus timelines. |
| 7 Nov | Theme fixed to **white and navy blue**, with no dark mode. |
| 7 Nov | The **Reviewer** is neutral but shares one console with the Developer. |
| 7 Nov | **XPT is used in Online Deposits** (confirmed by the product owner). |
| 7 Nov | **Plan change:** the one-page rule was withdrawn. The new direction is **one login portal with role-based consoles (RBAC)** for Customer, Contractor and Service Provider (Developer / Reviewer), with a collapsible, organized sidebar. Online Deposits is **postponed**. |
| 7 Nov | v7 published: the role portal. These docs were written to hand off the design. |

## 2. Phases

| Phase | What | Owner | Status |
|---|---|---|---|
| 1. Discovery | Read the slides, agree the roles, flows and scope | Design | ✅ Done |
| 2. Clickable demo | Login + 3 consoles, RBAC, timeline, notifications | Design (with Claude) | ✅ v7 live |
| 3. Design docs | PRD, AppFlow, UI_Spec, TRD, Schema, Plan | Design | ✅ This PR |
| 4. Review | Manager, investor and programmer feedback on the demo and docs | Manager, programmer | ⏳ Next |
| 5. Brand pass | Swap in the official SVG logo and exact brand colours | Design | ⏳ Waiting for assets |
| 6. Online Deposits | Preorder, rental and non-delivery flows using XPT | Design | 🅿️ Parked |
| 7. Build | Production app per the [TRD](TRD.md) | Engineering | Not started |

## 3. Suggested enhancements (backlog)

1. **Bond release needs both** developer acceptance and customer confirmation. ✅ (already in v7)
2. "?" help tips on XPT, bond, ponding test and reviewer for non-technical viewers.
3. A presenter-notes toggle, with one line to say per screen during an investor demo.
4. Pilot metrics that update from the demo run (acceptance time, dispute rate, bond reuse).
5. Upload real photos in the demo (file picker) instead of placeholder tiles.
6. A Bahasa Malaysia language toggle.
7. Online Deposits, as a fourth console area or a separate product (phase 6).

## 4. Open questions

- Exact XPT ↔ RM rate, and how contractors fund their XPT wallet.
- Who employs the Reviewer (Pegasuswork QA is assumed).
- Whether the customer gets any share of a deduction.
- Where XPT lives technically (own ledger, partner wallet or chain). See [TRD §4](TRD.md#4-xpt-bond-and-ledger-requirements).
- Official brand assets.

## 5. How to view the demo

- **Live artifact:** [XPTNOW Bond Demo](https://claude.ai/artifact/K73aknsXmo8UgSJqHWmBGE)
- **In this repo:** open [`demo/index.html`](../demo/index.html) in any browser. It needs no install or build.
- **Demo path (about 3 minutes):** sign in as Customer → Report an issue → user menu → Switch role → Developer → Customer reports → Create work order → Contractor → Job offers → Accept and lock → Active jobs → add both photos → Mark complete → Developer → Inspections → add sign-off → Accept → Customer → My repairs → Yes, I'm happy (or Raise a concern → Reviewer → Disputes → Approve).

---

## 6. Build prompt (exact)

This is the consolidated prompt that reproduces the v7 demo. Paste it into Claude (or another capable model) to rebuild it as one HTML file. It brings together everything that was asked for and agreed during the design sessions.

````text
Build a clickable investor demo for "XPTNOW Service Bonds" as ONE self-contained HTML file
(HTML + CSS + vanilla JavaScript, no framework, no build step). Fonts: Inter (UI) and
JetBrains Mono (numbers, IDs, times) from Google Fonts. Icons: small inline SVGs.
It is a showcase only: all data is mocked in memory, nothing is sent anywhere.

CONTEXT
Source brief: an investor slide "A contractor backs the repair with a bond":
  01 LOCK: contractor posts XPT, setting deadline, acceptance criteria and an RM-equivalent bond before work.
  02 VERIFY: developer inspects, capturing photos and sign-off. AI can flag missing evidence.
     A named reviewer handles disputes.
  03 RELEASE: return locked XPT after acceptance. Hold disputed amounts, apply agreed deductions.
  Pilot measures: acceptance time, dispute rate, bond reuse across jobs.
Show amounts as "1,500 XPT" with "≈ RM 1,500" (1 XPT ≈ RM 1, illustrative).

THEME (single light theme, no dark mode): white and navy blue only.
Tokens: bg #F5F7FB, surface #FFFFFF, surface-2 #F1F4FA, ink #0B1735, muted #5A6580,
line #E1E6EF, brand #1B3A8C, brand-hover #2A4FB0, brand-soft #E8EDF8, navy #0F1E45,
navy-2 #162A5C, ok #0F7A47/#E2F4EA, warn #9A5B00/#FFF1D9, bad #B4302A/#FDE6E4.
Logo: text wordmark "XPTNOW", weight 900, "XPT" in brand blue (light blue on navy).
Card radius 14px, buttons 10px. Very user friendly, plain language.

STRUCTURE
1) LOGIN PORTAL (one login for everyone). Left navy panel: logo, headline
   "Repairs backed by a bond, from report to release.", 3 numbered steps.
   Right: "Sign in", 3 role cards (Customer: unit owner or buyer; Contractor: fixes
   the work, locks the bond; Service Provider: developer or reviewer). If Service Provider
   is chosen, show a Developer / Reviewer toggle. Email pre-filled per role, any password works,
   button "Sign in as <Role>".
2) APP SHELL (role-based access control, RBAC). A navy sidebar that is functional and neatly
   organized into sections with expandable/collapsible headers; a collapse button shrinks it
   to icons only (tooltips, badges on icons); on phones it is an off-canvas drawer with a
   hamburger and scrim. Clicking an item changes the view dynamically. Amber badges show
   items that need the user. Top bar: portal name + page title, notifications bell with an
   unread count and a pop-over list, and a user menu with "Switch role (demo)" (all 4 roles),
   "Restart demo" and "Sign out". Toasts confirm every action.
   Menus per role:
   - Customer: [My home] Home, Report an issue, My repairs; [Support] Help & FAQ
   - Contractor: [Work] Home, Job offers, Active jobs; [Money] XPT wallet
   - Developer: [Overview] Overview; [Work] Customer reports, Jobs, Inspections;
     [Admin] Contractors, Roles & permissions
   - Reviewer (same Service Provider console, different rights): [Overview] Overview;
     [Work] Disputes (reviewer only), Jobs (view only); [Admin] Roles & permissions
3) ONE SHARED LIVE JOB flows through all roles: WO-24817, Unit B-12-03, Residensi Taman Melur.
   States: none -> reported -> assigned -> bonded -> completed -> inspected -> released,
   with disputed reachable from completed (developer disputes) or inspected (customer concern),
   and released from disputed (reviewer decision).
   - Customer reports an issue (area, issue type, description, photo tiles).
   - Developer opens Customer reports and creates a work order: contractor
     (Sinar Jaya Renovation / Kilat Builders), deadline, bond slider (default 1,500 XPT).
   - Contractor sees the job offer and presses "Accept and lock 1,500 XPT"
     (wallet: 3,500 available -> 2,000 available, 1,500 locked).
     Then uploads "After photo" and "24h ponding test" and presses "Mark repair complete".
   - Developer inspects: evidence tiles plus their own "site sign-off"; AI CHECK box lists
     missing items (amber) or "Evidence complete" (green); "Accept work" is enabled only when
     complete; or "Raise a dispute" (reason + requested deduction, default 300 XPT).
   - Customer: after inspection, "Yes, I'm happy" releases the bond; or "Raise a concern".
   - Contractor can reply to a dispute ("It is condensation, not a leak", etc.).
   - Reviewer sees Disputes with three side cards (who raised it, contractor reply, evidence),
     a deduction input, "Approve and release" or "Reject, return full bond".
   - Release: return (amount - deduction) to the contractor wallet, bond reuse +1,
     show a green/amber split bar.
   Never auto-switch roles or views after an action; only the user switches.
4) JOB TIMELINE on every console (right-hand card): vertical steps with time and actor;
   done = filled navy dot with ✓, current = ringed dot + "NOW", upcoming = hollow,
   optional = dashed; the viewer's own steps carry a "YOU" tag. Dispute steps appear only
   when a dispute exists. Every work screen also shows a "Waiting on: <role>" box that starts
   with "Your turn." for the owner.
5) EXTRA SCREENS: Customer home (unit card, "Protected by a 1,500 XPT bond" card),
   Help & FAQ (accordions: What is XPT? What is a bond? What if I'm not happy? Do I pay?).
   Contractor home (wallet stat tiles + "Needs your action"), XPT wallet (transactions).
   Developer overview (metric tiles: avg acceptance time 2.4 days, dispute rate 8%,
   bond reuse 3.1 jobs, open jobs; all labelled sample data), jobs table with filters
   (All/Active/Disputed/Released) and search, sample rows WO-24802, WO-24790, WO-24771.
   Contractors table. Roles & permissions matrix (rows = permissions, columns = 4 roles,
   current role highlighted).
6) NOTIFICATIONS: each hand-off notifies the next role(s) (see the AppFlow doc).
7) QUALITY: responsive down to 360px with no sideways page scroll (tables scroll inside
   their own container), visible focus states, Escape closes pop-overs, respect
   prefers-reduced-motion, empty states that explain what will appear.
Demo people: Aina Rahman (Customer), Sinar Jaya Renovation (Contractor),
Farid Hassan (Developer), Nur Aisyah, Pegasuswork (Reviewer). Footer note: demo only,
illustrative data, not connected to xptnow.ai.
````
