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

Rehosting:

1. **Router basename.** `BrowserRouter` was given `basename="/cf-cinebox"` so routing
   works under the repository subpath instead of the domain root.
2. **Asset paths.** Root-absolute references to fonts, logo, video and signature were
   rewritten to the subpath; they live in the JS and CSS, not only in `index.html`.

Data corrections:

3. `"Iron Claw"` corrected to `"The Iron Claw"`.
4. Cinematographer backfilled on five records where sibling scenes of the same film
   already carried the credit.
5. `"Criterion"` replaced as studio on seven films with the original distributor.
   Criterion is a home-video label, not a releasing studio.
6. **Theme taxonomy.** 93 loose mood tags consolidated into 18 canonical themes.
   41 of the originals appeared exactly once, and the set mixed era, subject, form
   and pure-visual terms in with actual moods. Every theme now carries 8-26 scenes.
7. **Theme cap removed.** The theme list was `.sort().slice(0,15)`, showing only the
   first fifteen names alphabetically and hiding the rest. It now derives from the data.

Interface:

8. **Empty-state heading.** "Select a Film" dropped `uppercase`, moved to `font-normal`,
   and went from `text-white/[0.05]` to `text-white/50` (1.08:1 to 5.32:1 against black).
   The compiled CSS carries a global `h1{text-transform:uppercase}`, so this also needed
   a `normal-case` utility that Tailwind had purged as unused.
9. **Chip grids** moved from four columns to three so theme labels stop clipping.
   `.grid-cols-3` was likewise purged and had to be appended.
10. **Typeface.** Geist replaced with the ABC Areal superfamily (sans and mono), served
    as variable fonts. Five static faces collapse into two. ABC Areal spans weight
    400-700, so the former 300 weight clamps to 400.

## Typeface licensing

ABC Areal is a commercial typeface from Dinamo Typefaces GmbH, included here at the
repository owner's direction. It is not a trial release. The font's embedded license
string excludes "storing on publicly available servers" and redistribution; this
repository is public. Anyone forking or reusing this repository needs their own license
from <https://abcdinamo.com/licenses>.

## Caveats

- Scene images load from the TMDB CDN and depend on it staying reachable.
- This build carries roughly 145 scenes. A later local build reached ~1,500 scenes but
  is not reproducible without the missing source files.
- Anything server-side in the original (auth, payments) is UI-only here.

## Rebuilding

Recovering a buildable project means restoring the missing source files. Until then,
treat this repository as an artifact, not a codebase.
