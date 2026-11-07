# #01 PRD: XPTNOW Service Bond Portal

| | |
|---|---|
| **Document** | Product Requirements Document |
| **Product** | XPTNOW Service Bond Portal (investor demo, design phase) |
| **Owner** | Zannat (UI/UX design) |
| **Status** | Draft for review |
| **Last updated** | 7 Nov 2026 |
| **Live demo** | [XPTNOW Bond Demo](https://claude.ai/artifact/K73aknsXmo8UgSJqHWmBGE) |

Related docs: [AppFlow](AppFlow.md) · [UI_Spec](UI_Spec.md) · [TRD](TRD.md) · [Schema](Schema.md) · [Plan](Plan.md)

---

## 1. Source context

This product was scoped from two slides of the XPT investor discussion deck. They are the only source brief for this design work.

![XPT investor discussion slides: Service Bonds and Online Deposits](assets/xpt-investor-slides.jpg)

What the slides say, in short:

**Service bonds: "A contractor backs the repair with a bond."** Pegasuswork can connect the bond to the work order and the inspection record.
1. **Lock:** the contractor posts XPT. They set the deadline, the acceptance criteria and a starting RM-equivalent bond before work begins.
2. **Verify:** the developer inspects. They capture photos and sign-off, AI can flag missing evidence, and a named reviewer handles disputes.
3. **Release:** return or deduct. The locked XPT is returned after acceptance. Disputed amounts are held, and agreed deductions are applied.
- *Pilot measure:* acceptance time, dispute rate, and bond reuse across jobs.

**Online deposits: "The deposit depends on who needs protection."** This section is **out of scope for now** (see section 9).

## 2. Problem

When a developer hires a contractor to fix a defect in a buyer's unit, three things often go wrong:
- The **customer** has no visibility into the repair and no guarantee it will be done properly.
- The **developer** has little leverage once the contractor has been paid, and inspection evidence is scattered across chats and photos.
- **Disputes** about quality have no neutral process, so they drag on.

## 3. Solution

A web portal where every repair is **backed by an XPT bond**. The contractor locks XPT before work starts. The XPT is returned only after the work passes inspection and the customer confirms. If there is a disagreement, a **neutral reviewer** decides how much (if anything) is deducted.

One login portal, with **role-based consoles (RBAC)**. Each person sees only what their role needs.

## 4. Users and roles

| Role | Who | Console | Main goal |
|---|---|---|---|
| **Customer** | Unit owner or buyer | Customer portal | Report a defect, follow the repair, confirm or raise a concern |
| **Contractor** | Renovation or repair company | Contractor portal | Win jobs, lock the bond, prove completion, get the bond back |
| **Developer** | Developer's defects or project team | Service Provider portal | Turn reports into work orders, assign contractors, inspect work |
| **Reviewer** | Neutral party (e.g. Pegasuswork QA) | Service Provider portal (same console, extra rights) | Decide disputes fairly |

The Developer and the Reviewer share **one Service Provider console**. The Reviewer sees an extra **Disputes** page, and the Developer never sees it. The Reviewer must be a different person from the Developer, because a party to a dispute cannot judge it.

## 5. Goals and success measures

| Goal | Measure (from the slides) |
|---|---|
| Faster sign-off on repairs | **Acceptance time:** from "repair completed" to "bond released" |
| Fewer, fairer disputes | **Dispute rate:** disputed jobs ÷ completed jobs |
| Capital efficiency for contractors | **Bond reuse:** number of jobs one locked amount is reused across |

## 6. Scope (this phase)

**In scope**
- Login portal with role selection.
- Customer console: home, report an issue, my repairs with a timeline, confirm or raise a concern, help & FAQ.
- Contractor console: home, job offers, lock bond, active jobs (upload proof, reply to disputes), XPT wallet.
- Service Provider console:
  - Developer: overview with metrics, customer reports, jobs table, inspections, contractors, roles & permissions.
  - Reviewer: overview, disputes, jobs (view only), roles & permissions.
- Shared job timeline and in-app notifications.
- Investor-demo helpers: "Switch role" menu and "Restart demo".

**Out of scope (this phase)**
- Online Deposits (preorder, rental, non-delivery). This is postponed.
- Real authentication, payments, XPT ledger integration and file storage. These are covered as requirements in the [TRD](TRD.md).
- Integration with the live xptnow.ai codebase. This design work is deliberately separate.

## 7. Functional requirements

| ID | Requirement | Role |
|---|---|---|
| FR-01 | Sign in on one portal; the role decides which console opens | All |
| FR-02 | Collapsible sidebar with grouped, expandable sections; clicking an item changes the view | All |
| FR-03 | Report a defect with area, issue type, description and photos | Customer |
| FR-04 | See that an active repair is protected by an XPT bond, with its amount | Customer |
| FR-05 | Confirm the repair, or raise a concern that goes to the Reviewer | Customer |
| FR-06 | Create a work order from a report: assign a contractor, set the bond amount, deadline and acceptance criteria | Developer |
| FR-07 | Accept a job offer by locking the required XPT from the wallet | Contractor |
| FR-08 | Upload completion proof (after photo, 24-hour ponding test) | Contractor |
| FR-09 | Inspect with an evidence checklist; the AI check flags missing evidence; accept only when complete | Developer |
| FR-10 | Raise a dispute with a reason and a requested deduction | Developer |
| FR-11 | Reply to a dispute with their side | Contractor |
| FR-12 | Decide a dispute: approve a deduction amount or reject; funds release immediately | Reviewer |
| FR-13 | Release the bond: full return, or return minus the deduction | System |
| FR-14 | Job timeline on every console with done, now and upcoming steps, and "YOU" tags | All |
| FR-15 | Notifications for each hand-off between roles | All |
| FR-16 | Jobs table with status chips, filters (All, Active, Disputed, Released) and search | Developer, Reviewer |
| FR-17 | Wallet: available, locked, bond reuse, transactions | Contractor |
| FR-18 | Roles & permissions page showing the RBAC matrix | Developer, Reviewer |

## 8. Business rules

1. A job cannot start until the contractor has **locked** the full bond.
2. The bond is released **only** after the developer accepts **and** the customer confirms, or after a reviewer decision.
3. Only the **Reviewer** can decide a deduction. The Developer and the Customer can only raise a dispute.
4. A deduction can never exceed the locked amount.
5. While a dispute is open, the bond stays locked.
6. Amounts are shown in XPT with an RM equivalent. The demo uses an illustrative 1 XPT ≈ RM 1.

## 9. Online Deposits (parked)

From the slides: preorder (buyer reserves stock), rental (renter covers damage) and non-delivery (seller backs delivery). On 7 Nov 2026 the owner confirmed that **XPT is meant to be used in online deposits**. This will be planned as a separate phase.

## 10. Assumptions and open questions

- **Assumption:** the "customer" is the unit owner or buyer in a development under a defect liability period.
- **Assumption:** the Reviewer sits at Pegasuswork. The slide says only "a named reviewer".
- **Assumption:** deducted XPT funds rework and goes to the developer.
- **Open:** the exact XPT to RM rate, and who funds the contractor's XPT wallet.
- **Open:** should the customer also get a share of a deduction?
- **Open:** where the real brand assets (SVG logo, exact hex codes) will come from.
