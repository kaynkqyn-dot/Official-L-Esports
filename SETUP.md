# L¹ Esports PWA — setup

This is the initial application foundation for L¹ Esports: Next.js + Supabase Auth/Postgres, black-and-gold responsive UI, English/Mongolian brand shell, sign-in/sign-up, player dashboard, and a database-backed tournament listing. The SQL includes initial tables and row-level security (RLS) policies.

## Important scope note
This is an early development starter, not a finished production tournament platform. It includes database-backed event pages, a signed-in 4-player team registration form, staff-only event creation/status management, and registration review. A Supabase project and SQL setup are required before the database works. Do not collect real player data until you have configured and tested the deployment.

## 1. Requirements
- Node.js 20+
- A Supabase account/project
- Git (recommended)
- Vercel account for hosting (recommended)

## 2. Configure Supabase
1. Create a new project at https://supabase.com.
2. In the Supabase dashboard, open **SQL Editor**.
3. Paste and run `supabase/schema.sql`.
4. Then paste and run `supabase/registration_hardening.sql` (normalized roster storage, atomic registration RPC, and slot capacity protection).
5. Open **Project Settings → API** and copy the Project URL and `anon` public key.
6. Copy `.env.example` to `.env.local`, then fill:
   ```
   NEXT_PUBLIC_SUPABASE_URL=https://YOUR_PROJECT.supabase.co
   NEXT_PUBLIC_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
   ```
7. Never place a Supabase `service_role` key in browser code or commit it to Git. This app uses the public anon key with RLS.

## 3. Run locally
```bash
npm install
npm run dev
```
Open http://localhost:3000.

## 4. Create the first administrator safely
1. Sign up with your own email on `/login`.
2. In Supabase dashboard → Authentication → Users, find your user UUID.
3. In SQL Editor, run this one time after replacing the UUID:
   ```sql
   update public.profiles
   set role = 'super_admin'
   where id = 'YOUR_AUTH_USER_UUID';
   ```
4. Sign out and sign in again, then open `/dashboard`.
5. Do not let users select their own role. New accounts are assigned `player` by the database trigger.

## 5. Deploy as a PWA
1. Push the project to a private/public Git repository.
2. Import it into Vercel.
3. Add the two environment variables from `.env.local` in Vercel project settings.
4. Deploy and connect your own domain.
5. Open the deployed HTTPS site on iPhone Safari and choose **Share → Add to Home Screen**. Android browsers typically offer **Install app**.

The manifest is included. Before production release, add real app icons, offline/service-worker behavior, proper email verification/redirect URLs, rate limiting and monitoring.

## 6. Production hardening checklist
- Test every RLS policy with anonymous, player, organizer, admin and super_admin accounts.
- Add server-side validated admin actions and audit logs.
- Add CAPTCHA/rate limiting for public sign-up.
- Configure Supabase Auth site URL and redirect URLs.
- Add backups, error monitoring, privacy policy and terms.
- Review data retention and consent before collecting team/player data.
- Keep all secrets server-side; never expose service-role credentials.
- Test mobile Safari and Android Chrome before public launch.

## Current implementation
- Responsive black/gold landing page and PWA manifest
- Supabase email/password sign-in and sign-up
- Role-aware dashboard
- Database-backed tournament list and tournament detail page
- Signed-in team registration form for a captain and four roster entries
- Atomic database RPC for team registration and normalized `registration_players` table (after running the hardening migration)
- Capacity checks to avoid concurrent submissions overbooking a tournament
- Staff-only admin page: create events, change status, review registrations
- Initial Postgres schema and RLS policies

### Still needed before public launch
- Edit/delete all event fields and fully editable homepage content/sections
- Real second-by-second countdown component
- Complete Mongolian/English translations throughout every page
- Bracket generation and editing, match result submission, audit logs
- Team logo upload/storage, profile settings and player dashboard registration status
- Friendly success/error notifications, email verification, spam protection/rate limiting
- Privacy policy, terms, backup/monitoring, and complete security testing
- Test all policies with anonymous, player, organizer, admin and super_admin accounts
- Confirm that tournament slot counts mean approved registrations only; pending submissions do not reserve slots in this starter

