# L¹ Esports — roadmap

## Product decision
Start with a mobile-first PWA website. Teams can open it from a phone browser and add it to their home screen. No App Store release is needed for the first version.

## MVP priorities
1. Connect Supabase and test email sign-in.
2. Run both SQL files in order: `schema.sql`, then `registration_hardening.sql`.
3. Give the organizer role to the intended admin account only from the Supabase dashboard/SQL.
4. Test the full flow: publish event → sign up → register four-player team → admin approves → public slot count updates.
5. Add editable event fields, homepage content editor, live countdown, and Mongolian/English switching.
6. Implement bracket generation/editing and result reporting.
7. Deploy to Vercel, connect the L¹ domain, and test on iPhone Safari and Android Chrome.

## Security notes
- Never expose the Supabase service-role key in client-side code.
- Never let users choose or edit their own role.
- The registration RPC creates team, membership, registration, and roster in one database transaction.
- Admin server actions check the signed-in user's role; RLS still needs real-account testing before launch.
- The project is a development starter and has not been deployed or production-audited.
