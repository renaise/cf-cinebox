# cf-cinebox

A static mirror of the deployed CineBox site, repackaged to run on GitHub Pages.

**Live:** https://renaise.github.io/cf-cinebox

## What this is

This is **not** the CineBox source. It is a captured copy of the compiled build that
was serving at `cinebox-10t.pages.dev`, repackaged so it can be hosted from GitHub
Pages at a subpath.

The original project source is incomplete on local disk — `package.json`, `index.html`,
`App.tsx`, `main.tsx` and several components and utils are missing — so the app cannot
currently be rebuilt from source. This mirror preserves the working build.

## Contents

- `index.html` — entry point, asset paths rewritten to `/cf-cinebox/`
- `404.html` — identical to `index.html`; GitHub Pages serves it for unknown paths so
  client-side routes (`/browse`, `/about`) survive a direct visit or refresh
- `assets/` — compiled JS and CSS, captured as served
- `favicon.svg`
- `.nojekyll` — disables Jekyll processing

## Modifications to the captured bundle

Two edits were applied to `assets/index-9LEfTrhp.js`:

1. **Router basename.** `BrowserRouter` was given `basename="/cf-cinebox"` so routing
   works under the repository subpath instead of the domain root.
2. **Empty-state heading.** The "Select a Film" heading dropped `uppercase` and moved
   from `font-medium` to `font-normal`.

## Caveats

- Scene images load from the TMDB CDN and depend on it staying reachable.
- This build carries roughly 145 scenes. A later local build reached ~1,500 scenes but
  is not reproducible without the missing source files.
- Anything server-side in the original (auth, payments) is UI-only here.

## Rebuilding

Recovering a buildable project means restoring the missing source files. Until then,
treat this repository as an artifact, not a codebase.
