# offtap-site

Static website for Offtap: the landing page and the privacy policy. Plain HTML and CSS. No JavaScript, no external requests (no fonts, scripts, analytics or images from elsewhere; the logo and favicon are inline SVG). Mobile-friendly, light and dark by system setting. Palette from the Offtap design system (rev 2026-10-27).

```
index.html            -> Play Console "Website" field
privacy/index.html    -> Play Console "Privacy policy" field (URL ends in /privacy/)
.nojekyll             -> tells GitHub Pages to serve the files as they are
README.md             (this file)
```

Hosting: **GitHub Pages**, from this repo's `main` branch root. Live address once enabled:

- Site: `https://yogirana5557.github.io/offtap-site/`
- Privacy policy (the URL for Play Console): `https://yogirana5557.github.io/offtap-site/privacy/`

All links in the pages are relative, so they work under the `/offtap-site/` sub-path. Keep it that way: never add a link that starts with `/`.

There is **no terms page** on purpose. The paywall design only shows a Terms link if `AppLinks.TERMS_URL` is non-null and it defaults to `null`. If you later publish terms, add `terms/index.html` and tell the Architect so a Builder run can set `TERMS_URL`.

## Keep the site informational (GitHub Pages terms)

GitHub says Pages "is not intended for or allowed to be used as a free web-hosting service to run your online business", or for a site "primarily directed at either facilitating commercial transactions or providing commercial software as a service" (docs.github.com, Pages limits, read 2026-10-26). A landing page and privacy policy for an app that people download from Google Play does not do that, but stay on the safe side: **no checkout, no payment or pricing forms, no sign-in.** The only outbound link may be the Google Play store link. If GitHub ever objects, Cloudflare Pages is a drop-in alternative that allows commercial use: connect this repo, framework "None", no build command, output directory `/`.

Pages on a free GitHub account needs a **public** repository. This repo holds only the two static pages, nothing secret. Do not put anything private in it.

## Deploy (free, about five minutes)

1. Push this repo to `https://github.com/yogirana5557/offtap-site.git` (branch `main`).
2. On GitHub: repo Settings, Pages, "Build and deployment": Source "Deploy from a branch", Branch `main`, folder `/ (root)`, Save.
3. Wait a minute, then open `https://yogirana5557.github.io/offtap-site/` and `.../privacy/` in a private window while logged out, and on mobile data. Both must load as normal pages (Play needs a publicly accessible, non-geofenced HTML page, not a PDF).

## Before you publish (owner)

1. `privacy/index.html`: the effective date ("27 October 2026") is the date the draft was written. Change it to the day you publish.
2. `index.html`: the line "Offtap is not on Google Play yet..." is true today. Replace it with the store link once the app is live. Do not link to the store before then.
3. **Do not add the sentence "Offtap does not have permission to use the internet" to either page until the release build proves it.** Play Billing 9.1.0 merges `android.permission.INTERNET` through Google's datatransport library; the R17 run removed it from the manifest, but that is a judgement call that still needs an ADR and a real licence-tester purchase test. The pages as written do not make the claim. When the release merged manifest has no `INTERNET` and a purchase still works, you may add to "What leaves your phone": "Offtap does not request the internet permission."
4. If the in-app review prompt ships (ADR 0006), add one sentence under "Google Play and Android": "If Offtap asks you to rate it, the rating card is shown and handled by Google Play."
5. Read both pages once as a user. Everything on them must still match the build you submit. If a feature or permission changes, change the page first.

## After it is live

1. Paste the privacy URL into Play Console (Policy and programs, App content, Privacy policy) and the website URL into Store settings, Contact details.
2. Tell the coordinator the URL. In the app repo, `AppLinks.PRIVACY_POLICY_URL` (`app/src/main/java/app/offtap/core/util/AppLinks.kt`) still holds `https://privacy.invalid/placeholder`; a Builder run must replace it (Release fails a build or listing that still has `.invalid` or `example.com`).
3. Update the Play Console prep file in the app repo (`pipeline/state/release/2026-10-27-play-console-prep.md`, placeholder rows).
