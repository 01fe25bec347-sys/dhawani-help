# Dhawani — GitHub Pages + Supabase starter

This package preserves the supplied Dhawani `index.html` and adds a secure database foundation for the full version.

## Important status

The supplied page is currently a hackathon prototype. Its account/session logic and submitted tickets use browser `localStorage`; the AI classification, photo analysis, department routing, and sample officials data are simulated. Publishing this file alone makes the prototype public, **not** a live multi-user grievance service.

The SQL in `supabase/schema.sql` creates the shared data model and row-level security policies. The frontend still needs to be connected to a Supabase project before citizen accounts, complaint submission/tracking, and official workflows become live. Do not describe the deployed prototype as an official government service.

## 1. Publish the current prototype on GitHub Pages

1. Create a GitHub repository named `dhawani` (public is simplest for GitHub Pages).
2. Upload `index.html`, `.nojekyll`, `README.md`, and the `supabase/` folder to the repository root.
3. In GitHub, open **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**.
5. Choose branch `main` and folder `/ (root)`, then Save.
6. Wait for the Pages deployment to finish. GitHub will show the public URL in Settings → Pages.

The public demo can be published before the backend is connected, but keep the prototype disclaimer visible.

## 2. Create the Supabase backend

1. Create a Supabase project.
2. Open **SQL Editor**, paste the contents of `supabase/schema.sql`, and run it.
3. In **Authentication → URL Configuration**, add your GitHub Pages URL to Site URL / Redirect URLs as appropriate.
4. In **Project Settings → API**, copy the Project URL and the `anon` / publishable key for the browser client.
5. Never put the `service_role` / secret key in `index.html`, JavaScript, GitHub, or a public repository.

## 3. Required frontend integration (next development step)

The next code change must replace the local demo account and ticket functions with Supabase Auth and database calls:

- Sign up / sign in / sign out using Supabase Auth (email verification; configure a real email provider before launch).
- Create and read citizen profiles.
- Insert complaints and fetch tickets from `public.complaints`.
- Track tickets by public `ticket_code` while limiting exposed personal information.
- Load the officials dashboard only for approved official accounts.
- Change complaint status through an authorized server-side workflow and write to `complaint_updates`.
- Upload photos to a private Storage bucket and use signed URLs; do not store raw image data in localStorage.

Do not use client-side role flags or a hidden admin button as authorization. Official approval must be performed by a trusted administrator through a protected workflow.

## 4. Before real public use

- Configure email verification and password reset.
- Add CAPTCHA / abuse protection and rate limits.
- Add privacy notice, retention policy, contact details, and a process for deleting user data.
- Test RLS using separate citizen, official, and unauthenticated accounts.
- Verify that one citizen cannot read another citizen's private details.
- Use real department/ward data only after validating it with the relevant authority.
- Keep the prototype / non-government disclaimer until formal authorization and integrations exist.

## Repository layout

```text
.
├── index.html             # Original Dhawani interface, preserved
├── .nojekyll
├── README.md
└── supabase/
    └── schema.sql         # Database tables, indexes, and RLS policies
```
