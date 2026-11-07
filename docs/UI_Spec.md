# #03 UI_Spec: XPTNOW Service Bond Portal

The visual and interaction spec for the portal. The [live demo](https://claude.ai/artifact/K73aknsXmo8UgSJqHWmBGE) is the reference implementation. If this doc and the demo disagree, flag it.

---

## 1. Design principles

1. **Very user friendly.** Plain words, one main action per card, and nothing hidden behind jargon.
2. **Each role sees only its own world.** Menus and actions come from RBAC, never from hiding things with CSS.
3. **Always show whose turn it is.** Every job shows its status chip and a "Waiting on: <role>" box.
4. **No surprise navigation.** Actions never switch the user to another role or console. Only the user switches.
5. **Trust through visibility.** The bond amount, the evidence and the timeline are always one click away.

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
| `--ok` / `--ok-soft` | `#0F7A47` / `#E2F4EA` | Released, accepted, AI check passed |
| `--warn` / `--warn-soft` | `#9A5B00` / `#FFF1D9` | Awaiting bond, AI check missing items, deductions |
| `--bad` / `--bad-soft` | `#B4302A` / `#FDE6E4` | In dispute, destructive actions |
| Sidebar badge | `#E2A23B` | "Needs you" counts |

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
| **Button** | Primary (navy fill), Ghost (white with border), Danger (white with red text and border), Small. Disabled at 45% opacity. |
| **Status chip** | Pill with a dot. Colour by state: blue = in progress, amber = awaiting bond, red = in dispute, green = released. |
| **Card** | White, 1px border, 14px radius. Header row: title on the left, chip on the right. |
| **Stat tile** | Small label, large mono number, one-line caption. |
| **"Waiting on" box** | Grey panel with the next-step sentence and a chip naming the owner role. Starts with **"Your turn."** if the viewer is the owner. |
| **Shield card** | Brand-soft panel with an icon. Used for "Protected by a N XPT bond" and the "Lock N XPT to accept" summary. |
| **Photo tile** | Dashed placeholder "+ Add photo" → solid green "✓ Added". Read-only tiles say "Not yet". |
| **AI check** | Amber box listing missing evidence, or a green "Evidence complete" box. |
| **Timeline** | Vertical list. ✓ filled navy dot = done (with time); ringed dot with a **NOW** tag = current; hollow = upcoming; dashed = optional. The viewer's own steps carry a **YOU** tag. |
| **Release split bar** | Green (returned) and amber (deducted) proportions, with a legend in XPT. |
| **Jobs table** | Columns: work order, unit, issue, contractor, bond, status, action. The live job row is tinted. Segmented filter and search above. |
| **Segmented control** | Used for filters and the Developer/Reviewer toggle on login. |
| **Notifications pop-over** | 340px; newest first; unread dot; "Today, HH:MM". Opening it clears the badge. |
| **User menu** | Switch role (demo), Restart demo, Sign out. |
| **Toast** | Navy, bottom centre, about 2.6s. Confirms every action ("Report sent to the developer"). |
| **Empty state** | Icon tile, bold title, one helpful sentence, and an optional single button. |

## 5. Screens

### Login
- Left: navy panel with the logo, headline "Repairs backed by a bond, from report to release.", and three numbered steps.
- Right: "Sign in", three role cards (Customer / Contractor / Service Provider), the Developer/Reviewer toggle when Service Provider is chosen, email (pre-filled per role), password, and the primary button **"Sign in as <Role>"**.

### Customer: Home, Report an issue, My repairs, Help & FAQ
### Contractor: Home, Job offers, Active jobs, XPT wallet
### Developer: Overview, Customer reports, Jobs, Inspections, Contractors, Roles & permissions
### Reviewer: Overview, Disputes, Jobs (view only), Roles & permissions

The content and order of each screen are listed in [AppFlow §6](AppFlow.md#6-screen-by-screen-flows). Layout rule: the **action card** goes on the left (main column) and the **job timeline** on the right (side column) on every work screen.

## 6. Copy guidelines

- Name things the way the user would. "Report an issue", not "Create ticket".
- Buttons say exactly what happens: "Accept and lock 1,500 XPT", "Send to reviewer", "Approve and release".
- Always show amounts as **"1,500 XPT"**, with "≈ RM 1,500" where money matters.
- Every empty state says what will appear and when.

## 7. Accessibility

- Visible focus ring (2px brand-2) on every control. Escape closes pop-overs and the drawer.
- Text meets WCAG AA contrast on both white and navy surfaces.
- Sidebar items have tooltips when collapsed. Icons are decorative (`aria-hidden`) and every button has a text label.
- Motion respects `prefers-reduced-motion`.
