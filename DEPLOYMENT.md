# NSSU PWA — Deployment Guide

## Files in this package

```
index.html       ← The NSSU app (renamed from the original)
manifest.json    ← Makes the app installable on Android
sw.js            ← Service worker — enables full offline use
icon-192.png     ← App icon (home screen, small)
icon-512.png     ← App icon (splash screen, large)
```

---

## Option 1 — GitHub Pages (Recommended, Free)

### One-time setup (~10 minutes)

1. Go to https://github.com and create a free account if you don't have one.

2. Click **New repository**. Name it something like `nssu-app`. Set it to **Public**.

3. Click **uploading an existing file** on the new repo page, then drag and drop
   all 5 files from this package.

4. Commit the files (click "Commit changes").

5. Go to **Settings → Pages** in your repo.
   - Under "Branch", select `main` and click **Save**.
   - GitHub will show you a URL like: `https://yourusername.github.io/nssu-app/`

6. That URL is your app. It's live, HTTPS, and ready to install.

### Installing on Android tablets

1. Open **Chrome** on the tablet and navigate to your GitHub Pages URL.
2. Wait for the page to fully load (this caches everything for offline use).
3. Tap the **three-dot menu (⋮)** in Chrome → **"Add to Home screen"** or
   **"Install app"**.
4. The app will appear on the home screen and open in its own window,
   just like a native app.

### Updating the app

When you make changes to `index.html` (or any file), just re-upload it to
GitHub and commit. The service worker will detect the change and update
automatically the next time the tablet has internet and opens the app.

---

## Option 2 — Cloudflare Pages (Also Free, Slightly Faster)

1. Go to https://pages.cloudflare.com and sign up.
2. Click **Create a project → Direct Upload**.
3. Upload all 5 files from this package.
4. Cloudflare will give you a URL like `https://nssu-app.pages.dev`.
5. Install on tablets the same way as above (Chrome → Add to Home screen).

---

## How offline mode works

- On **first load** (with internet), the service worker caches the entire app
  including the PDF libraries (html2canvas + jsPDF).
- After that, the app works **completely offline** — form filling, PDF export,
  JSON saving to local storage, and revision tracking all work with no signal.
- The JSON folder-write feature (saving to tablet subfolders) requires the
  tablet user to grant folder permission once while online. After that,
  the folder handle is stored in IndexedDB and reused automatically.

---

## JSON file saving — how it works on Android

The app saves two kinds of records:

| Type | Where it goes |
|------|--------------|
| **In-app records** | Browser localStorage (always works, offline or online) |
| **JSON files** | A folder the user picks on the tablet's storage |

On first Save or Initiate, Chrome will prompt the user to **pick a folder**.
They should pick or create a folder like `Documents/NSSU Builder Subfolder`.
The app automatically creates `Draft NSSUs` and `Initiated NSSUs` subfolders
inside that location.

The folder permission is remembered by IndexedDB — the user only needs to
grant it once per browser session. If Chrome is fully restarted, it may
re-prompt once on the next save.

---

## Troubleshooting

**"Add to Home screen" option doesn't appear**
→ Make sure you're using Chrome (not Samsung Internet or Firefox).
→ Make sure the URL is HTTPS (GitHub Pages and Cloudflare Pages both are).

**JSON files aren't being saved to the folder**
→ Check that the user tapped "Allow" when Chrome asked for folder permission.
→ The folder must be on accessible storage (Downloads, Documents, etc.),
  not a system-protected path.

**App shows old version after an update**
→ Open Chrome, go to the URL, and do a hard refresh (pull down to reload).
  The service worker will update in the background and apply on next open.
