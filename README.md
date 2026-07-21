# The Calabash — Spa & Salon Management Suite

A single-page spa, salon & health-club management app, built from the
**Calabash Spa Management** design. It's a self-contained `index.html` —
no build step, no dependencies, no server. Just open it in a browser.

## Run

Open `index.html` directly, or serve the folder:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Sign in

The app opens on a login screen. Two demo accounts (password **`spa123`**):

| Username   | Role         | Sees                                             |
|------------|--------------|--------------------------------------------------|
| `lucy`     | Manager      | Everything                                        |
| `beatrice` | Receptionist | All modules **except** Billing, Reports, Marketing, Analytics |

> **Note:** this is **demo/client-side** authentication only — accounts live in
> the page and the session is stored in the browser. It gates the UI and
> demonstrates role-based access; it is **not** secure server-side auth. For
> real accounts you'd need a backend (server + database + password hashing).

## Features

**Ten modules** (desktop sidebar):

| Area | Modules |
|------|---------|
| Operations | Dashboard · Appointments · Clients |
| Finance | Billing & Payments · **Taxes & EFRIS** · Inventory · Reports & Ledgers |
| Growth | Marketing · Analytics |
| Group | **Branch Performance** |
| Facilities | Staff Management · Gym & Health Club |

### Taxes & EFRIS (admin)

Tax overview tied to every transaction, modelled on Uganda's URA **EFRIS**
(Electronic Fiscal Receipting & Invoicing) with the standard **18% VAT**:

- KPIs — gross sales, VAT collected, net revenue, and EFRIS invoices issued today.
- EFRIS fiscal-receipt ledger — each transaction with gross / net / VAT, a
  Fiscal Document Number (FDN) and fiscalisation status.
- EFRIS device panel — TIN, taxpayer, device status, and fiscalised / queued /
  failed counts.
- Tax breakdown and **accumulated VAT** across daily, weekly, monthly, quarterly
  and annual periods. New bookings feed straight into these totals.

### Branch Performance (admin)

Group-wide view of all three branches (Wampewo · Munyonyo · Bukoto):

- A period switch — **Daily / Weekly / Monthly / Quarterly / Annually**.
- Group revenue, top branch, group VAT and branch count for the chosen period.
- A revenue-by-branch comparison chart and a league table with each branch's
  revenue, share of group, activity and trend vs the previous period.

Both admin modules are visible to the **Manager** role only.

**Working features** (persisted in the browser via `localStorage`)
- **Login / logout** with role-based module access (see above).
- **Real bookings** — the 5-step New Booking wizard actually creates the
  appointment: it's added to Today's Schedule and the Billing register (with a
  fresh receipt number), and the dashboard Revenue / Appointments counts update.
  Booking starts by choosing a **sector — Spa, Salon, Gym, or All Services**
  (combined cross-sector packages) — which filters the services/packages shown.
- **Add & edit clients** — a working form creates new clients and edits
  existing ones; changes survive a page reload.
- **Working search** — the client search box filters the list by name or phone
  as you type.

**Interactions**
- **Desktop / Mobile** view toggle (mobile renders an in-app phone frame).
- **Branch switcher** — Wampewo · Munyonyo · Bukoto (rebrands the header).
- **Dashboard** — live KPIs, today's schedule, on-shift staff, low-stock alerts.
- **Appointments** — colour-coded room schedule board + quick-add service menu.
- **Clients** — searchable list; click a row to open a full profile with
  loyalty points, preferences, allergies, upcoming appointments and service history.
- **New Booking wizard** — Service → Date & Time → Therapist & Room → Client
  details → Confirm & pay, with validation and an SMS-reminder toggle.
- **Billing, Inventory, Staff, Marketing, Analytics, Reports, Gym** — each a
  fully laid-out module with the suite's data.

## Resetting demo data

Bookings and clients you create are saved in the browser. To wipe them and
return to the seed data, clear the site's storage (DevTools → Application →
Local Storage → delete the `calabash_*` keys), or run in the console:
`localStorage.clear()`.

## Design

- **Palette** — warm stone `#e7e3da`, forest sidebar `#20261f`, sage accent
  `#3f5340`, gold `#c9b491`.
- **Type** — Cormorant Garamond (display) + Instrument Sans (UI).
- Currency and sample data are Uganda-based (Ugx).

## Structure

Everything lives in `index.html`:
- **Data** — plain JS constants (staff, clients, services, schedule, sales…).
- **Style helpers** — small functions returning inline style strings.
- **Views** — one render function per module returning an HTML string.
- **`App`** — event handlers that mutate state and call `render()`.

State changes trigger a full re-render of `#root`; text inputs in the booking
wizard keep focus across re-renders.
