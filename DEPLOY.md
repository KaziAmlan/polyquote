# Deploying PolyQuote to GitHub

You have two ways to publish the prototype. Pick ONE. The standalone HTML is the
fast path; the Vite route is the "fits my existing portfolio app" path.

---

## Option A — Standalone HTML (fastest, ~10 minutes)

The file `polyquote-prototype.html` is fully self-contained: it loads React and
Babel from a CDN and runs with no build step. GitHub Pages serves it as-is.

### A1. If you want a brand-new repo just for this
1. On GitHub, click **New repository**. Name it e.g. `polyquote`. Make it **Public**. Create it.
2. On the repo page, click **Add file → Upload files**.
3. Drag in `polyquote-prototype.html`, `polyquote-prototype.jsx`, `polyquote-prd.md`, and `README.md`. Commit.
4. Go to **Settings → Pages**. Under **Build and deployment → Source**, choose **Deploy from a branch**. Pick branch `main` and folder `/ (root)`. Save.
5. Wait ~1 minute. Your site appears at:
   `https://<your-username>.github.io/polyquote/polyquote-prototype.html`
6. Paste that URL into the README's "Live" line.

### A2. If you'd rather fold it into your existing `pm-prototypes` repo
1. In that repo, **Add file → Upload files** and drop the four files into a new
   folder, e.g. `polyquote/`.
2. If Pages is already on for that repo (it is, since the site is live), the
   prototype will simply be available at:
   `https://kaziamlan.github.io/pm-prototypes/polyquote/polyquote-prototype.html`
3. Add a link to it from your prototypes index page.

That's all — no build, no config.

---

## Option B — Add to your existing Vite/React app (cleaner, uses your pipeline)

Use this if `pm-prototypes` is a Vite (or CRA) app and you want PolyQuote to live
as a real route inside it, matching the rest of your portfolio.

### B1. Add the component
1. Copy `polyquote-prototype.jsx` into your app's source, e.g.
   `src/projects/PolyQuote.jsx`.
2. It already does `import { useState, useMemo, useEffect } from "react";` and
   `export default function PolyQuote()`, so it's a standard component — no edits needed.

### B2. Wire up a route
- **If you use React Router**, add:
  ```jsx
  import PolyQuote from "./projects/PolyQuote.jsx";
  // ...
  <Route path="/polyquote" element={<PolyQuote />} />
  ```
- **If your index is just a list of links**, import it where you render a project
  and show it on its own page/section.

### B3. Build and deploy
Run your normal commands — typically:
```bash
npm run build
npm run deploy      # if you use gh-pages
```
or push to the branch your Pages site builds from. PolyQuote will be live at
`https://kaziamlan.github.io/pm-prototypes/polyquote` (or whatever route you chose).

> **Vite base path gotcha:** for project Pages sites the app must know its subpath.
> In `vite.config.js`, confirm `base: '/pm-prototypes/'` is set, or assets 404.
> (If your existing prototypes already render correctly, this is already done.)

---

## The PRD

`polyquote-prd.md` needs no special handling — GitHub renders Markdown (including
the tables) automatically. It will display cleanly in the repo file view, and you
can link to it from the README (already wired) and from your prototype page.

---

## Recommended layout

```
polyquote/                         (or repo root)
├── README.md                      ← the case study (renders on the repo home)
├── polyquote-prd.md               ← the spec
├── polyquote-prototype.html       ← the no-build demo
└── polyquote-prototype.jsx        ← the component, for Option B
```

## Before you publish — quick checklist
- [ ] Add your live Pages URL to the README "Live" line.
- [ ] Keep the "illustrative demo" disclaimer at the top of the README (the duty
      figures and FR numbers are fabricated for the demo).
- [ ] Optional: replace "Pacific Rim Polymers" with any fictional name you prefer —
      it appears in the prototype's customer-facing screens.
- [ ] Optional: link the prototype from your main `pm-prototypes` index so it's discoverable.
