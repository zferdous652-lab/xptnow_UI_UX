# XPTNOW UI/UX: Service Bond Portal

The UI/UX design handoff for the **XPTNOW Service Bond Portal**: a portal where every repair is backed by an XPT bond that a contractor locks before starting and gets back once the work is accepted.

> **Design phase only.** This repo holds the design docs and a clickable demo. It is not production code and is not connected to xptnow.ai.

## See it

- **Live demo:** [XPTNOW Bond Demo](https://claude.ai/artifact/K73aknsXmo8UgSJqHWmBGE)
- **Offline:** download [`demo/index.html`](demo/index.html) and open it in any browser. No install needed.

**3-minute demo path:** sign in as **Customer** → Report an issue → *(user menu → Switch role)* **Developer** → Customer reports → Create work order → **Contractor** → Job offers → Accept and lock → Active jobs → add photos → Mark complete → **Developer** → Inspections → Accept → **Customer** → My repairs → *Yes, I'm happy* (or *Raise a concern* → **Reviewer** → Disputes → Approve).

## Who uses it (RBAC)

| Role | Console | Can do |
|---|---|---|
| Customer (unit owner) | Customer portal | Report issues, follow the repair, confirm or raise a concern |
| Contractor | Contractor portal | Accept jobs by locking XPT, upload proof, reply to disputes, see wallet |
| Developer | Service Provider portal | Create work orders, assign contractors, inspect, accept or dispute |
| Reviewer | Service Provider portal (extra rights) | Decide disputes and deductions |

## Docs (`/docs`)

| # | Doc | For | What's inside |
|---|---|---|---|
| 01 | [PRD.md](docs/PRD.md) | Manager, investor | Problem, users, scope, requirements, the source investor slides |
| 02 | [AppFlow.md](docs/AppFlow.md) | Everyone | Login and navigation, job state machine, happy and dispute flows (diagrams) |
| 03 | [UI_Spec.md](docs/UI_Spec.md) | Designer, programmer | Colours, type, layout, components, screens, copy |
| 04 | [TRD.md](docs/TRD.md) | Programmer | Recommended stack, RBAC, XPT ledger rules, non-functional requirements, API sketch |
| 05 | [Schema.md](docs/Schema.md) | Programmer | Data model (ER diagram), tables, permission matrix |
| 06 | [Plan.md](docs/Plan.md) | Manager | Decisions, phases, backlog, open questions, **the exact prompt used to build the demo** |

## Feedback

Please leave comments on the docs through a pull request or an issue. Brand colours and logo are placeholders until the official assets arrive. Online Deposits is planned for a later phase.
