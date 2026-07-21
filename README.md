# The Calabash — Spa & Salon Management Suite

A single-page spa, salon & health-club management app, built from the
**Calabash Spa Management** design. It's a self-contained `index.html` —
no build step, no dependencies, no server. Just open it in a browser.

## Run

Open `index.html` directly, or serve the folder:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Features

**Ten modules** (desktop sidebar):

| Area | Modules |
|------|---------|
| Operations | Dashboard · Appointments · Clients |
| Finance | Billing & Payments · Inventory · Reports & Ledgers |
| Growth | Marketing · Analytics |
| Facilities | Staff Management · Gym & Health Club |

**Interactions**
- **Desktop / Mobile** view toggle (mobile renders an in-app phone frame).
- **Branch switcher** — Wampewo · Munyonyo · Bukoto (rebrands the header).
- **Dashboard** — KPIs, today's schedule, on-shift staff, low-stock alerts.
- **Appointments** — colour-coded room schedule board + quick-add service menu.
- **Clients** — searchable list; click a row to open a full profile with
  loyalty points, preferences, allergies, upcoming appointments and service history.
- **New Booking wizard** — 5-step flow (Service → Date & Time → Therapist &
  Room → Client details → Confirm & pay) with validation, an SMS-reminder
  toggle, and a confirmation screen.
- **Billing, Inventory, Staff, Marketing, Analytics, Reports, Gym** — each a
  fully laid-out module with the suite's data.

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
