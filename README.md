# CWCT Trust Website

A responsive static website for Coimbatore Wildlife Conservation Trust (CWCT), with an optional Supabase-backed content manager.

## Files
- `index.html` — public website and admin sign-in/dashboard shell
- `styles.css` — responsive styling
- `app.js` — site rendering, forms, Supabase auth, content management and CSV export
- `config.js` — public configuration values
- `supabase-schema.sql` — database tables and row-level security policies

## Quick preview
Open `index.html` in a browser. Public sections and sample content render immediately. In local-only preview, forms save only in the same browser; the admin login is intentionally disabled without Supabase.

## Recommended: enable secure cloud backend
1. Create a Supabase project.
2. In Supabase **SQL Editor**, run the full `supabase-schema.sql`.
3. In **Project Settings → API**, copy the Project URL and publishable (or anon) key.
4. Put those values in `config.js` as `supabaseUrl` and `supabaseAnonKey`. These browser-safe values are intended for public frontend use; never paste a `service_role`/secret key into the site.
5. In Supabase **Authentication → Users**, create your admin account. Copy its user UUID.
6. In SQL Editor, grant admin rights:
   `insert into public.user_roles (id, role) values ('YOUR-AUTH-USER-UUID', 'admin');`
7. Publish the static files to GitHub Pages, Netlify, Cloudflare Pages or your own static host. The site must be served over HTTPS for production.
8. Test: submit a contact form, sign into Admin, add/publish a project, and confirm the public site displays it.

## Hosting
### GitHub Pages
Create a repository, upload all files in this folder to the repository root, enable **Settings → Pages**, and deploy from your chosen branch/root. `config.js` must be included.
### Other hosting
Upload all five files to the same public folder. No Node.js build step is required.

## Security and limitations
- Static website files are public. Never store private keys, admin passwords or service-role keys in them.
- Supabase Row Level Security is intended to keep unpublished content and submissions protected. Review the policies before real production use and add rate limiting / CAPTCHA or an edge function if you expect spam.
- Admin management currently supports adding, publishing/unpublishing and deleting projects, activities and gallery entries. Submissions can be reviewed and exported to CSV.
- Gallery images are currently entered as public image URLs. To use direct uploads, configure a Supabase Storage bucket and implement bucket policies before enabling upload controls.
- Donation page is intentionally a contact/support CTA; no payment processor is configured.
- Replace/verify sample text, dates, images, official contact information and any organizational claims before launch. The seed activity list is based on known CWCT activity descriptions, but should be reviewed by the trust.
- The no-Supabase local preview is not secure, not multi-device synced, and not suitable for collecting real personal information.


## Full website CMS admin panel

- Open `admin.html` to use the separate CWCT website content management panel.
- Configure the Supabase project URL and browser-safe publishable/anon key in `config.js`.
- Run the complete `supabase-schema.sql` in the Supabase SQL Editor (the CMS adds `site_settings`).
- Create an Authentication user and insert its UUID into `public.user_roles` with role `admin` before signing in.
- Use **Page content** to edit individual text fields and button links. Use **Brand & design** to set the logo URL, favicon URL, hero/join images, colors, page title and SEO description. Save changes to publish the content settings.
- Use the other admin tabs to add/delete projects, activities, gallery items, members, wildlife reports, donations and documents, and export supported records as CSV.
- Media fields currently use image/document URLs; direct file upload and drag-and-drop page building are not included in this version.
- Changes to site settings are stored in Supabase and loaded by the public site. Public records require published=true.
