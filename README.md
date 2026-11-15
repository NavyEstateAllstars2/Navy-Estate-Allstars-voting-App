# Football Awards Hub

A mobile-responsive football awards voting application starter with voter accounts, administrator-only management, nominee image uploads, one-vote-per-category enforcement, and private results.

## Important security design
- Voters can read categories and nominees, and submit their own votes.
- The `votes` table has **no voter SELECT policy**. Only admins can read vote rows.
- A database unique constraint enforces one vote per user per category.
- A database trigger validates that voting is open and that the nominee belongs to the selected category.
- The app never queries results for voter accounts.
- Admin privileges are assigned manually in the database, never through public sign-up.

## Setup
1. Create a project at Supabase.
2. In Supabase SQL Editor, run all of `supabase/schema.sql`.
3. Register your own account in the app (after starting it).
4. In Supabase SQL Editor, promote that account:
   ```sql
   update public.profiles
   set role = 'admin'
   where id = (select id from auth.users where email = 'YOUR-ADMIN-EMAIL');
   ```
5. Copy `.env.example` to `.env` and add your Supabase project URL and anon/public key.
6. Install dependencies and run:
   ```bash
   npm install
   npm run dev
   ```
7. For production, run `npm run build` and deploy the project to Vercel. Add the same two environment variables in Vercel.

## Supabase Auth settings
- Enable email/password authentication.
- Configure the production Site URL and redirect URLs in Authentication > URL Configuration.
- Keep email verification enabled for production.
- Do not expose the Supabase service-role key in the browser or in `.env` variables prefixed with `VITE_`.

## Admin setup
The first admin is promoted with the SQL command above. To add more admins, update the corresponding profile role from `voter` to `admin` in the Supabase SQL editor.

## Notes before public launch
- Add your own privacy policy, terms, support contact and branding.
- Configure Supabase SMTP for reliable verification emails.
- Review local legal requirements and data retention practices.
- Consider rate limiting and CAPTCHA for public sign-ups.
- A production audit should be performed before a high-profile public award.
