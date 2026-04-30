# NSSU Input Portal

A self-contained, installable web app (PWA) for building NSSU Input Forms. Provides autofill suggestions, signature capture, and PDF export for completed forms.

The entire app ships as a single `index.html` file with inline CSS and JavaScript — no build step, no bundler, no backend. Drop it on any static host (or open it locally) and it works.

## Features

- **Single-file app** — all UI, logic, and styling live in `index.html`.
- **PWA** — installs to home screen, works offline after first load via service worker.
- **Autofill suggestions** — blue suggestion text is inserted into Step and Rework fields based on the selected defect, project type, and area. Clicking into a field clears the suggestion so the user can start fresh.
- **Manual mode** — fully manual entry path for fields where the autofill doesn't apply.
- **Signature capture** — touch/mouse signature pads for sign-off.
- **PDF export** — renders the completed form to a multi-page PDF.
- **Local persistence** — drafts and edit history are stored in `localStorage`.
- **Edit report** — every manual change to a Step or Rework box is logged with full context and exportable as JSON.
- **Library override** — the suggestion library can be replaced at runtime via "Update Library" in the menu.

## File layout

```
.
├── index.html          # The entire app (HTML + CSS + JS, ~5,800 lines)
├── manifest.json       # PWA manifest
├── sw.js               # Service worker (offline cache)
├── icon-192.png        # PWA icon, 192×192
├── icon-512.png        # PWA icon, 512×512
├── README.md
├── CHANGELOG.md
├── LICENSE
└── .gitignore
```

That's the whole project. There is no `node_modules`, no build output, no `dist/` directory.

## Running locally

The app needs to be served over HTTP for the service worker to register. Opening `index.html` directly via `file://` will work for the form itself but the PWA / offline behavior won't kick in.

Pick whichever you have installed:

```sh
# Python 3
python3 -m http.server 8080

# Node
npx serve .

# PHP
php -S localhost:8080
```

Then visit `http://localhost:8080/`.

## Deploying

Any static host works. Recommended options:

### GitHub Pages

1. Push this repo to GitHub.
2. **Settings → Pages → Build and deployment → Source:** "Deploy from a branch".
3. **Branch:** `main` (or `master`), **Folder:** `/ (root)`.
4. Save. The site will be available at `https://<user>.github.io/<repo>/` within a minute or two.

### Netlify / Vercel / Cloudflare Pages

Connect the repo, leave the build command blank, set the publish directory to the repo root. No environment variables needed.

### Internal web server / SharePoint / file share

Drop all files (`index.html`, `manifest.json`, `sw.js`, both icons) into the same directory on the server. The relative paths in `index.html` will resolve correctly.

## Updating

When you ship a new build:

1. Edit `index.html`.
2. **Bump `CACHE_VERSION` in `sw.js`** (e.g., `nssu-v1.0.0` → `nssu-v1.0.1`). This forces existing installs to drop the old cache and pull the new files. If you skip this step, users running the installed PWA may keep seeing the old version until they manually clear cache.
3. Commit and push.

Optionally update `CHANGELOG.md`.

## Browser support

Targets evergreen Chrome, Edge, Firefox, and Safari (desktop and mobile). The signature pad uses Pointer Events with a touch fallback. PDF export uses the embedded jsPDF/html2canvas pipeline inside `index.html`.

## Hidden / disabled features

- **Import from Work Instruction** — the menu button (`#importWiBtn`) is hidden via inline `display:none` while the underlying importer logic remains in place. To re-enable, remove the `style="display:none;"` attribute from the button on or near line 264 of `index.html`.

## License

See [LICENSE](./LICENSE).
