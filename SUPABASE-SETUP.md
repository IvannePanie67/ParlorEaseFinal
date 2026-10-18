# ParlorEase — Calm Green Edition

The app now runs on **Supabase** (Postgres database + Supabase Auth). Data is no longer kept in the browser's `localStorage`.

## Run locally
1. Open `index.html` in a browser, or use VS Code Live Server.
2. `config.js` already points at the ParlorEase Supabase project. To use your own project, change `SUPABASE_URL` and `SUPABASE_PUBLISHABLE_KEY` there (Supabase Dashboard → Project Settings → API).
3. Never put a `service_role` / secret key in this project. The publishable key is safe in the browser because Row Level Security protects every table.

## Database setup (new Supabase project)
1. Supabase Dashboard → **SQL Editor → New query**.
2. Run `supabase-schema.sql`.
3. Run `supabase/migrations/002_app_integration.sql`.
4. (Optional, for classroom demos) **Authentication → Providers → Email**: turn off *Confirm email* so new customers can log in immediately.

## Accounts and roles
- **Customer** — anyone can create an account from the app. A `profiles` row and a `customers` row are created automatically.
- **Admin** — create an account in the app, then promote it in the SQL editor:
  ```sql
  update public.profiles p set role = 'admin'
  from auth.users u where p.id = u.id and lower(u.email) = lower('your-admin@email.com');
  ```
- **Staff** — the staff member first creates an account in the app. Then, as admin, open **Staff → Add Staff** (or edit an existing staff record) and enter their login email. The app calls `link_staff_account`, which gives that account the staff role and links it to the staff record.

## What each role can do
- **Admin** — manage customers, services, staff, appointments, queue, transactions and reports.
- **Staff** — see appointments assigned to them and mark them completed.
- **Customer** — browse services, request appointments (pending until an admin approves), and cancel their own.

## Tables
- `profiles` — Auth-linked identity and role
- `customers` — customer contact details and visit count
- `staff` — staff directory and availability
- `services` — service catalog, prices, durations, and image URLs
- `appointments` — bookings and statuses (a staff member can't have two active bookings in the same slot)
- `transactions` — payments and receipts
- `service_queue` — queue entries, priority, and state

RLS is enabled on all tables. Review and test policies before using real customer data.

## Final-project checklist from the supplied criteria
- Functional interface and distinct customer/staff/admin experiences
- Add, update, delete, and display operations for management pages
- Search and sorting controls on data tables
- Validation on forms and constrained database fields
- Persistent storage in Supabase (Postgres) with Supabase Auth
- DSA explanation should still be prepared separately for at least four data structures and four algorithms, including where each is used and why it was selected.
