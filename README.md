# littleGallery site

Three static pages — landing, privacy policy, terms — served by GitHub Pages at
the app's own subdomain. No build step, no Jekyll (`.nojekyll` is there to say
so), no JavaScript.

```
index.html      landing page, with the support section the stores link to
privacy.html    served at /privacy — the URL the Play listing requires
terms.html      served at /terms — subscription terms, cancellation, liability
404.html
assets/style.css   the app's palette (see lib/core/theme/app_palette.dart)
assets/icon.png    the launcher icon, copied from the app
CNAME           the custom domain, one line, no scheme
```

GitHub Pages resolves an extensionless path to the matching `.html` file, so
`/privacy` and `/terms` are the URLs to hand out — they are what the pages
declare as canonical and what the app and the store listings link to. The
`.html` forms still answer, so any link already in the wild keeps working.

## Publishing

1. Create a **public** repo on GitHub — `littlegallery-site` is a good name —
   and push this folder to `main`:

   ```powershell
   git init -b main
   git add -A
   git commit -m "littleGallery site"
   git remote add origin https://github.com/<you>/littlegallery-site.git
   git push -u origin main
   ```

2. **Settings → Pages**: source *Deploy from a branch*, branch `main`, folder
   `/ (root)`.

3. **Settings → Pages → Custom domain**: enter the subdomain and save. That
   writes the same value as `CNAME`; keeping the file means the setting
   survives a force-push.

4. At your DNS provider, add a **CNAME record** for the subdomain pointing at
   `<you>.github.io.` (apex domains need A records instead; a subdomain does
   not). Propagation is usually minutes.

5. Wait for the certificate, then tick **Enforce HTTPS**. Play will not accept
   a privacy policy URL that fails to load over HTTPS.

## Keeping it in step with the app

The legal copy is duplicated from `app/legal/*.md`, which is what the release
checklist points at. When one changes, change both — the app's settings drawer
links to these pages by URL, so they must say the same thing.

After the site is live, put the two URLs into
`app/lib/features/settings/settings_drawer.dart` (`privacyUrl`, `termsUrl`) and
into the Play Console listing.
