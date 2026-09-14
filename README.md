# Income Management, installable PWA

A standalone copy of the ledger, built to run as a real home-screen app on iPhone.
Every figure is stored in the browser's `localStorage` on the device. Nothing is
sent anywhere: there is no server component, no analytics, no network call after
the page has loaded.

## Why this exists

As a published Claude artifact the page runs inside a cross-site iframe. WebKit
deliberately makes storage in that position ephemeral, so on iOS the numbers were
discarded whenever Safari fully quit. Served from your own origin the page is
first-party, and once added to the Home Screen it is also exempt from Safari's
7-day cap on script-writable storage.

## Deploy to GitHub Pages

```bash
# in this folder
git init
git add .
git commit -m "Income Management PWA"
git branch -M main
git remote add origin git@github.com:<you>/<repo>.git
git push -u origin main
```

Then: **Settings → Pages → Source: Deploy from a branch → `main` / `/ (root)` → Save.**

The site appears at `https://<you>.github.io/<repo>/` within a minute or two.
All paths here are relative, so the subdirectory URL works with no changes.

## Install it on the iPhone

1. Open the Pages URL **in Safari** (not Chrome, not an in-app browser).
2. Tap **Share**, then **Add to Home Screen**.
3. Launch it from the icon from then on.

Launched from the icon it runs full-screen with its own storage, works offline,
and keeps the figures indefinitely. The page shows a reminder about this on iOS
until it is installed or the reminder is dismissed.

## What is in here

| Path | What it is |
|---|---|
| `index.html` | the whole app, one file |
| `manifest.webmanifest` | name, icons, standalone display |
| `sw.js` | service worker, caches the shell for offline use |
| `vendor/xlsx.full.min.js` | SheetJS 0.18.5, vendored so export works offline |
| `fonts/*.woff2` | Fraunces, IBM Plex Sans, IBM Plex Mono, latin subset |
| `icons/*.png` | home-screen and maskable icons |
| `.nojekyll` | stops GitHub Pages running the files through Jekyll |

## Changed from the artifact version

- Excel export uses an ordinary object-URL download instead of Claude's
  `downloads` capability, which does not exist outside an artifact.
- Fonts and SheetJS are served locally rather than from a CDN, so a cold
  offline launch renders and exports correctly.
- The "Safari discards data in this embedded view" warning is gone; it no
  longer applies.
- Added: web app manifest, service worker, icons, and the iOS install reminder.

## Updating it later

Edit `index.html`, bump `CACHE` in `sw.js` (`income-management-v1` → `-v2`),
commit and push. Installed copies pick up the new version on the next launch
while online.

## A note on the password screen

The repo and the page are publicly readable, as any GitHub Pages site is. The
lock screen is a privacy screen for the device, not account security, and anyone
reading the source could bypass it. That does not expose your numbers: they only
ever exist in your own browser's storage, never in the repo and never on the
server. Use **Export to Excel** now and then if you want a backup that survives
losing or wiping the phone.
