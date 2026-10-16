# Personal academic website

Hand-written static site for Elie Attal (PhD student, CMAP / École Polytechnique),
published by GitHub Pages at <https://eattal.github.io>.

No framework, no build step, no static site generator: plain HTML files plus one
stylesheet. Keep it that way — the site must stay editable by hand without a toolchain.

## Layout

| File | Page |
| --- | --- |
| `index.html` | About (bio, photo, contact links) |
| `research.html` | Preprints and publications |
| `talks.html` | Talks, split into "Conferences and workshops" and "Seminars" |
| `teaching.html` | Teaching, grouped by academic year |
| `style.css` | All styling |
| `cv/cv.tex` | LaTeX source of the CV |
| `cv.pdf` | Compiled CV at the site root — what the nav links to |
| `.nojekyll` | Tells GitHub Pages to serve the files as-is |

The nav block is duplicated in every page's `<header>`. When adding a page, add the
link to all of them and give the current page `aria-current="page"`.

## Entry markup

Publications and teaching entries:

```html
<li>
  <span class="title">Title</span>
  <span class="meta">coauthors &middot; venue &middot; year</span>
  <span class="links"><a href="…">arXiv</a><a href="…">Journal</a></span>
</li>
```

Talks lead with the event, not the talk title:

```html
<li>
  <span class="title"><a href="…">Event name</a><span class="tag">invited</span></span>
  <span class="meta">City, Country · date</span>
  <span class="talk"><em>Title of the talk</em></span>
  <span class="note">Winner of the QRT PhD Prize</span>
  <span class="links"><a href="…">Slides</a></span>
</li>
```

- `.tag` renders a small outlined badge; drop it for non-invited talks.
- `.note` is prefixed with a gold star by CSS — don't type the star.
- `.links` and `.note` are optional; delete the line when unused.
- Remote talks are marked by `(online)` in the place field.

## Conventions

- Dates: month first, en dash in ranges — `September 17–18 2026`, `3 May 2025`.
- Separator between items: `&middot;` with a space on **both** sides.
- Accents and special characters go in directly (`é`, `ø`, `&oslash;`), never as
  LaTeX escapes.
- Math uses `$…$`; KaTeX is loaded on every page, so a literal dollar sign must be
  written `\$`.
- Most recent first within each section.

## Working on the site

Preview locally:

```bash
python3 -m http.server 8321
```

then open <http://localhost:8321>. After a CSS change the browser pane may serve a
cached stylesheet — force a reload.

Rebuild the CV after editing `cv/cv.tex`:

```bash
cd cv && pdflatex cv.tex && cp cv.pdf ../cv.pdf
```

The copy at the site root is what gets published; `cv/cv.pdf` is gitignored. In this
template, a line starting with `[` right after `\\` must be written `{}[…]` or LaTeX
fails with "Missing number".

## Publishing

Pushing to `main` deploys; the live site updates within about a minute. There is no
CI and no pull request workflow.

```bash
git add -A && git commit -m "…" && git push
```
