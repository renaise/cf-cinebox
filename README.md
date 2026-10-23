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

8. **Every frame re-sourced (2026-10-21).** Only 19 of 145 scenes showed a frame from the
   film named on the card. The rest reused a few unrelated Are.na images: one frame from
   *A Most Violent Year* stood in for 45 different films, one from *Made in U.S.A.* for 37.
   Each scene now points at a still from that film's FilmGrab gallery and carries a `source`
   field linking the post. A film was matched only when the FilmGrab title and year agreed
   with the record, and its director was checked as well. All 140 frames were then checked by eye.
   No two scenes share a frame.
9. **Five scenes removed.** *Climax*, *A Separation*, *Bottoms*, *Nyad* and *Rustin* have no
   FilmGrab gallery, so no frame could be sourced.
10. **Timestamps cleared.** The `timestamp` values were not derived from anything. The UI
    already hides an empty one.
11. **Duplicate IDs.** IDs 101-105 were each used twice; the second of each is renumbered.

12. **Distributors verified (2026-10-23).** `studio` means the film's original US
    distributor. Each value was checked against the film's Box Office Mojo domestic
    distributor (looked up by IMDb ID through Wikidata), and against Wikipedia where Mojo
    has no entry. Twelve were corrected: *Amélie* → Miramax, *Portrait of a Lady on Fire* → Neon,
    *The Handmaiden* → Magnolia Pictures, *Memories of Murder* → Palm Pictures,
    *The Neon Demon* → Broad Green Pictures, *Burning* and *Monster* → Well Go USA,
    *A Girl Walks Home Alone at Night* → Kino Lorber, *Blue Velvet* → De Laurentiis
    Entertainment Group, *Possession* → Limelight International, *Napoleon* → Columbia
    Pictures, *Evil Does Not Exist* → Janus Films. *Stalker* (Mosfilm) and *Cure* (Daiei)
    have no recorded original US release and carry the producing studio.

Interface:

13. **Empty-state heading.** "Select a Film" dropped `uppercase`, moved to `font-normal`,
   and went from `text-white/[0.05]` to `text-white/50` (1.08:1 to 5.32:1 against black).
   The compiled CSS carries a global `h1{text-transform:uppercase}`, so this also needed
   a `normal-case` utility that Tailwind had purged as unused.
14. **Chip grids** moved from four columns to three so theme labels stop clipping.
   `.grid-cols-3` was likewise purged and had to be appended.
15. **Typeface.** Geist replaced with the ABC Areal superfamily (sans and mono), served
    as variable fonts. Five static faces collapse into two. ABC Areal spans weight
    400-700, so the former 300 weight clamps to 400.
16. **Marquee slowed.** Each row's loop went from 300s to 600s, about 30px/s on desktop.
    The two rows are disjoint halves of the scene list, so no frame appears in both.
17. **Distributor shown.** Each film card in Browse carries its distributor on its own line,
    and the marquee hover reads director · year · distributor.
18. **Distributor logos.** The text line "A24 · Criterion · Neon · and more" became a row of
    the eight distributors with the most scenes: A24, Searchlight, Neon, Warner Bros.,
    Paramount, Netflix, IFC Films and Focus Features. Criterion no longer appears in the data.
    The SVGs are from Wikimedia Commons and render white through a CSS filter. Warner uses
    the single-color file so the shield's cut-out survives the filter.
19. **Search caret removed.** A decorative `|` sat in front of the hero search input.

## Typeface licensing

ABC Areal is a commercial typeface from Dinamo Typefaces GmbH, included here at the
repository owner's direction. It is not a trial release. The font's embedded license
string excludes "storing on publicly available servers" and redistribution; this
repository is public. Anyone forking or reusing this repository needs their own license
from <https://abcdinamo.com/licenses>.

## Caveats

- Scene images hotlink film-grab.com and depend on it staying reachable and allowing it.
- This build carries 140 scenes. A later local build reached ~1,500 scenes but
  is not reproducible without the missing source files.
- Anything server-side in the original (auth, payments) is UI-only here.

## Rebuilding

Recovering a buildable project means restoring the missing source files. Until then,
treat this repository as an artifact, not a codebase.

20. Each Browse card now carries a row directly under the title: the distributor's logo (name as text where no clean logo exists, including Sony Pictures, Weinstein and GKIDS, whose marks are illegible at card size), the runtime, and the number of stills of that film in the collection. The still count is computed from the scene data, not stored. Runtimes come from each film's English Wikipedia infobox (first listed, theatrical cut), cross-checked against Wikidata; 17 more logos come from Wikimedia Commons, cropped to their drawn bounds. "Magnolia" was merged into "Magnolia Pictures".
