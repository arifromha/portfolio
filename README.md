# Fana Journalist Intelligence Platform (Fana JIP) — v0.1

Bilingual English/Amharic web application starter for Fana Media Corporation.

## Included in this first version

- React + Vite responsive web interface
- English / Amharic language switch
- Dashboard and navigation for all planned modules
- Preview mode with sample research records
- Research item creation UI
- Supabase email/password authentication hook
- Supabase PostgreSQL schema and initial row-level security policies
- Journalist, editor and administrator role model
- Mobile-responsive layout

## Not yet connected

This is a functional first-version frontend and database foundation, not a finished integration of every third-party product. Feedly, Google Alerts, Otter.ai, Whisper processing, Google Lens, Datawrapper, Tableau, Power BI and OpenRefine need separate API/service configuration and backend workers. The module workspaces currently provide the initial UI and workflow scaffolding.

## Requirements

- Node.js 20+
- A Supabase project
- Netlify account (or another static web host)

## Run locally

```bash
npm install
cp .env.example .env
# Fill in VITE_SUPABASE_URL and VITE_SUPABASE_ANON_KEY
npm run dev
```

Open the local URL printed by Vite.

If `.env` is not configured, the app opens in Preview mode using sample data. Do not use preview mode for real newsroom data.

## Supabase setup

1. Create a Supabase project.
2. Open SQL Editor.
3. Run `supabase/schema.sql`.
4. Enable Email authentication.
5. Create the first user in Supabase Authentication.
6. Copy that user's UUID and promote the account to admin:

```sql
update public.profiles
set role = 'admin'
where id = 'YOUR_AUTH_USER_UUID';
```

7. Add the Supabase project URL and anon/public key to `.env`.

Never put the Supabase `service_role` key in frontend environment variables. It must only be used by trusted backend functions.

## Netlify deployment

- Build command: `npm run build`
- Publish directory: `dist`
- Add environment variables:
  - `VITE_SUPABASE_URL`
  - `VITE_SUPABASE_ANON_KEY`

For client-side routing in later versions, add a Netlify redirect rule:
`/* /index.html 200`

## Internal + online access design

The same hosted application can be reached from office and remote networks. For internal-only resources, use Supabase RLS, private storage buckets, authenticated access, and optionally an organization VPN or reverse proxy/network policy. Do not rely on hiding a URL or frontend navigation as access control.

## Recommended production architecture

Browser (React) → Supabase Auth → PostgreSQL (RLS) + private Storage → Supabase Edge Functions / trusted API service → approved external integrations.

Keep third-party API secrets in server-side environment variables. Add audit logging, backups, retention policies, and a newsroom source-protection policy before production.

## Important editorial and security safeguards

- Treat AI transcripts and summaries as drafts requiring human verification.
- Preserve original audio, original files and source URLs where permitted.
- Do not upload confidential source identities to external AI providers without authorization.
- Apply least-privilege access and test RLS policies with journalist/editor/admin test accounts.
- Confirm legal, licensing and procurement requirements for external services.
- Review Amharic, Afaan Oromoo and Tigrinya transcription quality before relying on automated transcripts.
