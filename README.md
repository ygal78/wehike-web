# wehike-web

Generated static build of the [we hike](https://github.com/) app's web
export, published via GitHub Pages at `wehike.authentic.gg`. This is build
**output**, not source — the real app lives in the separate `we-hike`
repo/project (Expo Router + Supabase). Don't hand-edit files here.

Only the Plan and Profile screens are meaningfully usable on web — Navigate's
GPS recording needs a native build (see the main repo's README).

## Redeploying after a change

From the `we-hike` project:

```bash
npx expo export -p web
```

Then copy the fresh `dist/` output over this repo's contents (keeping
`CNAME` and `.nojekyll`), commit, and push:

```bash
cp -r ../we-hike/dist/. .
cp index.html 404.html   # SPA fallback — see below
git add -A
git commit -m "Update build"
git push
```

## Why the extra files

- **`CNAME`** — GitHub Pages custom domain. Must contain exactly
  `wehike.authentic.gg`.
- **`.nojekyll`** — GitHub Pages runs Jekyll by default, which ignores any
  folder starting with `_` — including Expo's `_expo/` bundle folder. Without
  this file the site's JS silently 404s while `index.html` loads fine.
- **`404.html`** — a copy of `index.html`. expo-router uses real (history-mode)
  URLs on web, but GitHub Pages has no server-side rewrite rule, so a direct
  link like `/trail/<id>` 404s on a plain static host. Serving the same app
  bundle for unmatched paths lets the client-side router take over and render
  the right screen from `location.pathname`.

## DNS

A CNAME record for `wehike` under `authentic.gg`, pointing at
`<github-username>.github.io` — set once when this repo is created and Pages
is enabled with the custom domain.
