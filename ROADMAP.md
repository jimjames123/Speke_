# Roadmap — from prototype to production SaaS

Calabash Cloud is currently a **client-side multi-tenant prototype**: one
`index.html` where onboarding, businesses (tenants), users and data all live in
the browser's `localStorage`. That's ideal for demos, pitching and shaping the
product — but it is **not** yet a real service, because:

- Each browser/device has its **own** copy of the data (nothing is shared).
- Anyone viewing source can read the demo passwords — there's **no real security**.
- There's no billing, no email, no backups, no audit trail.

This document is the plan to turn it into production software that multiple real
businesses can pay for and rely on. Phases are ordered so each one ships value.

## Phase 1 — Backend + shared database (the core leap)
- **Stack:** Node.js (NestJS or Next.js API routes) + **PostgreSQL**. Prisma/Drizzle for the schema.
- **Multi-tenancy:** a `tenants` table; every row in `clients`, `bookings`,
  `sales`, `users`, `branches` carries a `tenant_id`. Enforce isolation with
  row-level security (or a scoped query layer) so one business can never read
  another's data.
- **Migrate the current models 1:1** — the shapes already exist in the prototype
  (tenant, user, client, booking, service, branch, VAT line).
- Replace every `localStorage` read/write with API calls; the UI stays largely as-is.

## Phase 2 — Real authentication & roles
- Server-side auth (email + password with **bcrypt/argon2** hashing, or an
  identity provider like Auth0/Clerk/Supabase Auth). JWT or session cookies.
- Email verification, password reset, invite-a-teammate.
- Proper RBAC: platform-owner, business-owner/admin, manager, receptionist,
  therapist — enforced on the **server**, not just hidden in the UI.

## Phase 3 — Billing & subscriptions
- **Stripe** (or Flutterwave/Pesapal for Uganda) subscription plans
  (e.g. Starter / Growth / Multi-branch), free trial, per-branch or per-seat pricing.
- Plan gating (e.g. Branch Performance & advanced reports on higher tiers),
  dunning, invoices, VAT on the subscription itself.

## Phase 4 — Real EFRIS / URA integration
- Replace the simulated fiscalisation with the **actual URA EFRIS API**:
  register invoices, receive real FDNs and verification QR codes, handle the
  offline queue and retries. Per-tenant TIN and device credentials, stored encrypted.

## Phase 5 — Notifications & integrations
- SMS reminders (Africa's Talking / Twilio) and email (Postmark/SES) — the
  reminder toggle already in booking becomes real.
- Calendar sync, WhatsApp Business, payment links for deposits.

## Phase 6 — Hosting, ops & hardening
- Deploy API + DB on a managed host (Render/Railway/Fly/AWS); managed Postgres
  with automated **backups** and point-in-time recovery.
- Custom domains per tenant (e.g. `spa-name.calabash.cloud`), CDN, logging,
  error tracking (Sentry), uptime monitoring.
- Security review, rate limiting, data-export & GDPR/consent, tenant offboarding.

## Phase 7 — Per-tenant depth (currently shared demo content)
In the prototype, reference lists (staff directory, room list, product stock,
gym timetable, marketing campaigns, analytics samples) are **shared demo
content** across businesses. In production each becomes fully per-tenant and
editable, plus:
- Per-tenant branding (logo, colours, currency & locale — the app is Ugx-only today).
- Hotel-specific modules (rooms, housekeeping, F&B) for hospitality customers.
- Configurable service catalogue & pricing per business.

---

### Suggested first milestone
Stand up Phase 1 + 2 (Postgres schema, tenant isolation, real login) behind the
existing UI, deploy to a managed host, and migrate the demo Calabash tenant as
seed data. That single milestone converts the prototype into a genuine —
if minimal — SaaS that real businesses can log into safely.
