# Decarbonisation News — Scan Desk (Render deploy)

Static dashboard for the decarbonisation.news scan desk. Reads candidate stories, renders them as tiles by section, and fires a Make webhook when you click Generate. Deploys to Render as a static site, the same pattern used for EVLife.

## Files

- `index.html` — the dashboard (single file, no build step)
- `render.yaml` — Render Blueprint, static site config

## Deploy via Terminal

### Step 1 — go to the folder

In Terminal, type `cd` followed by a space, then drag the `decarbonisationnews-scan-desk` folder from Finder onto the Terminal window and press Enter. That fills in the exact path for you.

### Step 2 — commit and push

If you have the GitHub CLI (`gh`), paste this whole block. It creates the repo and pushes in one go, no username needed:

```bash
git init
git add .
git commit -m "Decarbonisation News scan desk"
git branch -M main
gh repo create decarbonisationnews-scan-desk --public --source=. --remote=origin --push
```

If you do NOT have `gh`, first create an empty public repo named `decarbonisationnews-scan-desk` on github.com (do not add a README), then paste this:

```bash
git init
git add .
git commit -m "Decarbonisation News scan desk"
git branch -M main
git remote add origin https://github.com/dshales-cell/decarbonisationnews-scan-desk.git
git push -u origin main
```

## Deploy on Render

Render and GitHub are already open in Chrome. Either route works.

Blueprint route (uses render.yaml):
1. Render dashboard, New, Blueprint.
2. Connect the `decarbonisationnews-scan-desk` repo. Render reads `render.yaml` and creates the static site.
3. Deploy. Copy the live URL (something like `https://decarbonisationnews-scan-desk.onrender.com`).

Manual route (no yaml):
1. Render dashboard, New, Static Site.
2. Connect the repo.
3. Build command: leave blank. Publish directory: `.`
4. Create, then copy the live URL.

Auto-deploy is on by default, so future `git push` updates the site.

## After deploy: wire it live

Open `index.html` and set the two constants near the top, then push again.

```js
const DATA_SOURCE = "";       // your candidate sheet: Google Sheets "Publish to web" CSV link, or a JSON endpoint
const GENERATE_WEBHOOK = "";  // the Make webhook that starts article + image generation
```

Until these are set, the desk runs on built-in sample data and Generate fires in demo mode.

## Notes

- Static hosting is enough: the Generate button POSTs straight to the Make webhook from the browser, so no backend is needed on Render.
- Custom domain (e.g. desk.decarbonisation.news) can be added later in Render, Settings, Custom Domains.
