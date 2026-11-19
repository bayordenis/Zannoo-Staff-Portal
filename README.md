# Zannoo School Portal: deployment guide

## Files
| File | Purpose |
|---|---|
| `portal/index.html` | The portal (login, dashboard, all modules) |
| `portal/config.js` | Your Supabase URL, public key and school id |
| `supabase/schema.sql` | Database tables and access rules (run once) |
| `supabase/create-admin.sql` | Creates your school and first admin (run once) |
| `website/enquiry-widget.js` | Sends the website's contact/admission forms to the portal |
| `deploy/github-pages-workflow.yml` | Optional automatic GitHub Pages deploy |
| `school-portal.html` | Browser-only demo (no database) |

## Steps
1. **Supabase project.** Create one at supabase.com (free tier). Authentication > Providers > Email: turn OFF "Allow new users to sign up".
2. **Database.** SQL Editor: run `supabase/schema.sql`.
3. **First admin.** Authentication > Users > Add user (email + password, tick Auto Confirm). Edit the email in `supabase/create-admin.sql`, run it, and copy the `school_id` it returns.
4. **Config.** Fill `portal/config.js` (URL and anon key from Project Settings > API, plus the school_id).
5. **Website forms.** Put the same three values in `website/enquiry-widget.js`, add it to your site (`<script src="/assets/enquiry-widget.js"></script>`), and mark each form as described in the file's header.
6. **Publish.** Add the `portal/` folder (and the widget) to your website repo and push. In GitHub: Settings > Pages > deploy from the main branch (or use the workflow file). Open `/portal/`.
7. **Test.** Sign in with the admin email. Add a student and reload on another device: it should still be there. Submit a website enquiry and check More > Enquiries.
8. **Add staff.** Create each user in Authentication > Users, then add a profile (see the end of `create-admin.sql`).

## Before real pupil data
- Free Supabase projects have no automatic backups: export the `school_data` and `enquiries` tables regularly (Table Editor > Export).
- Keep `supabase/` and `deploy/` out of the public repo if you can; they hold no secrets but there is no need to publish them.
- Never put the `service_role` key anywhere in these files.
- Roles are not fully separated yet (admin, teacher and HR have the same access) and there are no parent logins.
- Collections are saved as whole blocks, so two people editing the same section at once can overwrite each other.
- Review consent and storage rules under Ghana's Data Protection Act.
