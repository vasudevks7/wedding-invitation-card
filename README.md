# Fahad & Nesrin — Wedding Invitation

## Project files
- `index.html` — home page
- `invite.html` — invitation, countdown, music and guest RSVP form
- `admin.html` — authenticated admin dashboard
- `config.js` — Supabase project URL and publishable key
- `setup.sql` — creates the RSVP table and Row Level Security policies
- `serve.js` — local development server
- `music.mp3` / `music.wav` — supplied audio files

## 1. Configure the new Supabase project
Project ref: `ifipdhqyvhrkfqmkxqpp`  
Project URL already set in `config.js`: `https://ifipdhqyvhrkfqmkxqpp.supabase.co`

1. Open Supabase Dashboard and select project `wedding`.
2. Go to **Project Settings → API Keys**.
3. Copy the **publishable key** (starts with `sb_publishable_`).
4. Open `config.js` and replace `PASTE_YOUR_SUPABASE_PUBLISHABLE_KEY_HERE` with that key.
5. Do NOT use a `service_role` or secret key in `config.js`.

## 2. Create the database
1. In Supabase, open **SQL Editor → New query**.
2. Paste all contents of `setup.sql`.
3. Click **Run**.
4. Confirm `public.wedding_rsvps` exists in Table Editor.

The SQL creates the table with:
- guest_name (required)
- attending (required boolean)
- message (optional)
- rating (optional, 1–5)
- created_at (automatic timestamp)

Guests can INSERT responses. Only the authenticated admin email `fahadnrfahad@gmail.com` can SELECT or DELETE them.

## 3. Create the admin login
In Supabase Dashboard:
1. Open **Authentication → Users**.
2. Add/invite a user with email `fahadnrfahad@gmail.com`.
3. Set a strong password and confirm/verify the user if prompted.
4. Use that email and password on `admin.html`.

The website does not create admin users. Do not put an admin password in any HTML or JavaScript file.

## 4. Run locally
Open this folder in VS Code. In the terminal:
```bash
node serve.js
```
Open:
- http://localhost:3000/
- http://localhost:3000/invite.html
- http://localhost:3000/admin.html

Keep the terminal running while testing.

## 5. Test before deployment
1. Submit a test RSVP from `invite.html`.
2. Check Supabase → Table Editor → `wedding_rsvps` for the new row.
3. Sign in to `admin.html` using the admin Auth account.
4. Confirm the row appears; test refresh, CSV export and delete.
5. Delete the test row after testing.

A successful on-screen RSVP confirmation means Supabase accepted the insert. If submission fails, the page shows an error rather than claiming it was saved.

## 6. Deploy
Upload the entire folder to your static hosting provider (for example, Vercel). Ensure `config.js`, both HTML files, and the music files are included. Do not deploy only `index.html`.

## Security notes
- The publishable key is intended for browser use; RLS is what protects the table.
- Never expose the Supabase secret/service_role key.
- The admin email check in the page is a UI restriction; database RLS independently enforces admin-only reads and deletes.
- Do not enable public SELECT or DELETE policies.
