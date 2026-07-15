# SETUP — Installation guide (admin)

For whoever publishes the app. End users only need HOW-TO-USE.md.

## 1. Create the Supabase project
1. supabase.com → New project (free plan is fine)
2. **Authentication → Sign In / Up:** make sure "Email" is enabled (magic link / OTP is on by default)
3. **Authentication → URL Configuration:** set *Site URL* to the app's final URL (e.g. `https://your-app.vercel.app/` or `https://<user>.github.io/dive-logbook/`)

## 2. Create the table (SQL Editor → New query → Run)
```sql
create table public.dives (
  user_id uuid not null references auth.users(id) on delete cascade,
  id text not null,
  payload jsonb not null,
  updated_at timestamptz not null default now(),
  primary key (user_id, id)
);

alter table public.dives enable row level security;

create policy "select own" on public.dives for select using (auth.uid() = user_id);
create policy "insert own" on public.dives for insert with check (auth.uid() = user_id);
create policy "update own" on public.dives for update using (auth.uid() = user_id);
create policy "delete own" on public.dives for delete using (auth.uid() = user_id);
```
The RLS policies guarantee each user can only read/write their own dives.

## 3. Connect the app
1. Supabase → Settings → API: copy the **Project URL** and the **anon public key**
2. In `index.html`, search for `SUPABASE_URL` and `SUPABASE_KEY` (near "Nuvem (Supabase)") and paste the values
   - The anon key is public by design — security comes from the RLS policies, not from hiding the key
3. Commit to the repo

## 4. Publish
**Vercel:** import the GitHub repo (vercel.com → Add New → Project). A static `index.html` at the root needs no configuration — every push to `main` auto-deploys.
**GitHub Pages (alternative):** repo → Settings → Pages → Deploy from a branch → `main`.

Make sure the published URL matches the Site URL from step 1.3 — otherwise magic links will redirect to the wrong place.

## Notes
- With `SUPABASE_URL`/`SUPABASE_KEY` left empty, the app runs in local mode (no accounts) — useful for testing.
- Supabase free tier limits (500 MB DB) are enough for thousands of dives with compressed photos.
- The ✨ AI features (stamp/logbook reading) only work when the app runs inside the Claude preview — on the published site they show a notice instead.
