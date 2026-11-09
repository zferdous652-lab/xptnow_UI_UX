# #03 UI_Spec: XPTNOW Escrow Marketplace

The visual and interaction spec for the portal. The [live demo](https://claude.ai/artifact/K73aknsXmo8UgSJqHWmBGE) is the reference implementation. If this doc and the demo disagree, flag it.

---

## 1. Design principles

1. **Very user friendly.** Plain words, one main action per card, and nothing hidden behind jargon.
2. **Each role sees only its own world.** Menus and actions come from RBAC, never from hiding things with CSS.
3. **Always show whose turn it is.** Every job shows its status chip and a "Waiting on: <role>" box.
4. **No surprise navigation.** Actions never switch the user to another role or console. Only the user switches.
5. **Trust through visibility.** The points on hold, who holds them (XPTNOW), the proof and the timeline are always one click away.
6. **XPT points only.** No RM or other currency appears anywhere.

## 2. Theme and tokens

Single **light** theme. **White and navy blue** only (no dark mode). These values are approximations from the xptnow.ai site and should be replaced when the official brand assets arrive.

| Token | Value | Use |
|---|---|---|
| `--bg` | `#F5F7FB` | App background |
| `--surface` | `#FFFFFF` | Cards, top bar, inputs |
| `--surface-2` | `#F1F4FA` | Inner panels, table headers |
| `--ink` | `#0B1735` | Main text |
| `--muted` | `#5A6580` | Secondary text |
| `--line` | `#E1E6EF` | Borders, dividers |
| `--brand` | `#1B3A8C` | Primary buttons, active states, links |
| `--brand-2` | `#2A4FB0` | Hover |
| `--brand-soft` | `#E8EDF8` | Selected backgrounds, info boxes |
| `--navy` / `--navy-2` | `#0F1E45` / `#162A5C` | Sidebar, hero banners, login panel |
| `--ok` / `--ok-soft` | `#0F7A47` / `#E2F4EA` | Released, verified, POD added |
| `--warn` / `--warn-soft` | `#9A5B00` / `#FFF1D9` | Quote sent, claim filed, refund share of a split |
| `--bad` / `--bad-soft` | `#B4302A` / `#FDE6E4` | In dispute, decline, refund all, reject |
| Sidebar badge | `#E2A23B` | "Needs you" counts, rating stars |

**Typography**
- **Inter** (400 to 900) for all UI text, and **JetBrains Mono** for numbers, amounts, IDs and timestamps (tabular figures).
- Scale: page title 19px/800, card title 16.5px/700, body 14.5px/400, labels 11.5px uppercase with 0.08em letter-spacing, notes 12.5px.

**Shape and spacing**
- Card radius 14px, buttons and inputs 10px, chips fully rounded.
- Card padding 20px (16px on phone). Grid gaps 18px.
- Shadows only on pop-overs (menu, notifications) and toasts.

**Logo:** a text wordmark, "XPT" in brand blue (light blue on navy) and "NOW" in ink (white on navy), weight 900, tight letter-spacing. To be replaced by the official SVG.

## 3. Layout

```
┌────────────┬──────────────────────────────────────────────┐
│  SIDEBAR   │  TOP BAR: portal name / page title · bell · user menu
│  (navy)    ├──────────────────────────────────────────────┤
│  logo  «   │  CONTENT (max 1240px)                        │
│  user card │   ┌────────── main (1.6fr) ─────┐┌ side (1fr) ┐
│  SECTION ▾ │   │ action card                 ││ timeline   │
│   item  ①  │   └─────────────────────────────┘└────────────┘
│   item     │                                              │
│  SECTION ▾ │                                              │
│  Sign out  │                                              │
└────────────┴──────────────────────────────────────────────┘
```

- **Sidebar:** 252px expanded, 76px collapsed (icons only, with the label as a tooltip; badges move to the icon corner). The collapse state is remembered per browser.
- **Breakpoints:**
  - **≤1100px:** two-column content stacks; stat rows go to 2 columns.
  - **≤900px:** the sidebar becomes an off-canvas drawer (272px) behind a hamburger, with a dim scrim; the login becomes one column.
  - **≤560px:** role cards stack; stats go to 2 small columns.
- Tables scroll sideways inside their own container. The page itself never scrolls sideways.

## 4. Components

| Component | Spec |
|---|---|
| **Button** | Primary (navy fill), Ghost (white with border), Danger (white with red text and border), Small. Disabled at 45% opacity. Link button (underlined brand text) for "Register your business". |
| **Status chip** | Pill with a dot. Amber = quote sent or claim filed, blue = points on hold / proof uploaded / release requested, red = in dispute, green = released, refunded or settled. |
| **Card** | White, 1px border, 14px radius. Header row: title on the left, chip on the right. |
| **Stat tile** | Small label, large mono number, one-line caption. |
| **XPT ID pill** | Mono ID on the navy greeting banner with a white "Copy" button. |
| **Escrow box** | Navy gradient panel with a lock icon: "500 XPT held safely by XPTNOW" plus one line on when the provider is paid. Shown on every open job. |
| **Flow line** | Small grey step chips joined by arrows (Approve quote → Points on hold → Provider uploads proof → You request release). |
| **"Waiting on" box** | Grey panel with the next-step sentence and a chip naming the owner role. Starts with **"Your turn."** if the viewer is the owner. |
| **Profession card** | Role-card style tile with an icon, label and examples. Six options, 3 columns (2 on tablet, 1 on phone). |
| **Step indicator** | Three pills (1. Profession · 2. Business details · 3. Submitted): navy = current, green = done. |
| **Provider card** | Initials logo, name, profession and location, verified badge, rating, job count, "from" price, "Ask for a quote". |
| **Verified badge** | Green check + "Verified". Pending providers show an amber "Pending" chip instead. |
| **POD tile** | Dashed placeholder "+ Add" → solid green "✓ Added". Read-only tiles say "Not yet". Both tiles are required before "Upload POD and mark done" is enabled. |
| **Claim countdown** | Three day bars that fill as days pass after POD, with "Skip 1 day (demo)". "File a claim" is enabled on day 3. |
| **Price field** | Number input with "XPT" suffix; out-of-range values show a toast with the profession's limits. |
| **Star rating** | Five large stars (amber when selected), shown after the job closes. |
| **Queue item** | Row with a tinted icon, kind, one-line description and an "Open" button. Used in the Reviewer's priority queue. |
| **Timeline** | Vertical list. ✓ filled navy dot = done (with time); ringed dot with a **NOW** tag = current; hollow = upcoming. The viewer's own steps carry a **YOU** tag. |
| **Outcome bar** | For a split: green (paid to provider) and amber (refunded to customer) proportions, with a legend in XPT. |
| **Tables** | All jobs, escrow ledger, providers. Segmented filter and search above; the live job row is tinted. |
| **Notifications pop-over** | 340px; newest first; unread dot; "Today, HH:MM". Opening it clears the badge. |
| **User menu** | Switch role (demo), Restart demo, Sign out. |
| **Toast** | Navy, bottom centre, about 2.6s. Confirms every action ("Quote sent to Aina Rahman"). |
| **Empty state** | Icon tile, bold title, one helpful sentence, and an optional single button. |

## 5. Screens

### Login
- Left: navy panel with the logo, headline "Pay for services with XPT points, safely held until the job is done.", and three numbered steps.
- Right: "Sign in", three role cards (Customer / Service Provider / Reviewer), email (pre-filled per role), password, and **"Sign in as <Role>"**. Under Service Provider: "New provider? **Register your business**".

### Register (Service Provider)
- Step 1 "What do you do?": six profession cards (Contractor, Mobile & electronics repair, Shop owner / Retailer, Dealer, Rental, Freelancer / Other). Continue is enabled once one is picked.
- Step 2 "Your business": the chosen profession and its XPT price limit, then business name, SSM registration number, location and business email (pre-filled in the demo). **Submit for review** or Back.
- Step 3 "Application sent": the business is **pending review** until an XPTNOW reviewer verifies it. Quotes can be sent only after approval.

### Customer: Home, Find a provider, Quotes, My services, XPT wallet, Help & FAQ
### Service Provider: Dashboard, My services, Send a quote, Jobs, XPT wallet, Business profile
### Reviewer: Priority queue, Release requests, Claims, Disputes, Escrow ledger, Provider approvals, All providers, All jobs, Roles & permissions

The content and order of each screen are listed in [AppFlow §6](AppFlow.md#6-screen-by-screen-flows). Layout rule: the **action card** goes on the left (main column) and the **job timeline** on the right (side column) on every work screen.

## 6. Copy guidelines

- Name things the way the user would. "Report an issue", not "Create ticket".
- Buttons say exactly what happens: "Approve and hold 500 XPT", "I'm satisfied, request release", "File a claim", "Apply split".
- Always show amounts as **"500 XPT"**. Never show RM.
- Say "on hold" and "held by XPTNOW", not "deposit" or "payment", until the points are released.
- Every empty state says what will appear and when.

## 7. Accessibility

- Visible focus ring (2px brand-2) on every control. Escape closes pop-overs and the drawer.
- Text meets WCAG AA contrast on both white and navy surfaces.
- Sidebar items have tooltips when collapsed. Icons are decorative (`aria-hidden`) and every button has a text label.
- Motion respects `prefers-reduced-motion`.
