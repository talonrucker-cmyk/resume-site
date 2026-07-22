# Talon Rucker — personal finance website

Static site published by GitHub Pages from `talonrucker-cmyk/resume-site`.
Live at https://talonrucker-cmyk.github.io/resume-site/

## Design rule (overrides normal instincts)

**Fewest words possible.** Talon's stated goal for the whole site is simplicity.
When a sentence can be cut, cut it. Prefer a visual or a number over a paragraph.

## Publishing workflow — read this before editing

`index-BACKUP.html` is the **canonical** file. `index.html` is the published copy.
The `/push` command runs `cp index-BACKUP.html index.html`, so **any edit made only
to `index.html` gets silently destroyed on the next push.**

**Always apply edits to BOTH files**, or edit `index-BACKUP.html` and let `/push`
copy it across. Verify with `cmp index.html index-BACKUP.html` (silence = identical).

`/push` also mirrors to `C:\Users\Jason\Downloads\index.html`. Website is canonical;
never copy Downloads → Website.

Never stage `.claude/` (local tooling). Never stage `index1.html` (old, unused).

## Files

| File | What it is |
|---|---|
| `index.html` | Published site — 4 pages in one file, toggled by JS |
| `index-BACKUP.html` | Canonical edit source (see above) |
| `lesson-protection.html` | Standalone lesson deck — insurance |
| `images/` | Photo gallery assets |
| `why-you-need-a-financial-planner.pdf` | Embedded guide |
| `EDITS.md` | The outstanding work list, batched |
| `../Backups/` | Dated safety copies, outside the repo |

## index.html section map

Line numbers drift as edits land — treat as approximate and confirm with Grep.
**Read only the range you need.** The file is 155KB; a full read is ~45k tokens.

| Lines | Section |
|---|---|
| 10–540 | `<style>` — all CSS |
| 551–570 | Header / nav (4 page tabs) |
| 572–599 | Hero (name, tagline, CTA buttons) |
| 600–636 | `#explore` — jump cards |
| 637–662 | `#creds` — education, licenses, designations, chips |
| 663–681 | `#about` |
| 682–750 | Compound interest teacher + chart |
| 752–844 | `#experience` — resume/job history |
| 847–1075 | `#tools` — calculators + `#balancesheet` (~1150) |
| ~1204 | `#bsModal` — balance-sheet pop-up; fields come from `GROUPS` in the script |
| 1076–1097 | `#guide` — embedded PDF |
| 1099–1116 | `#contact` |
| 1118–1558 | **Page 3: Tools & Demos** — six numbered subsections |
| 1126–1256 | ├ 1 · Protection |
| 1257–1361 | ├ 2 · Cash Flow |
| 1362–1415 | ├ 3 · Liquidity & Assets |
| 1416–1442 | ├ 4 · Tax & Debt |
| 1443–1527 | ├ 5 · Retirement (Monte Carlo ~1478) |
| 1528–1558 | └ 6 · Estate |
| 1560–1700 | **Page 4: Fun** — gallery, Spotify, books, tutorials |
| 1700–2195 | `<script>` — all JS (Monte Carlo ~2147) |
| 2197–2210 | Photo lightbox |

## Page toggle

The four nav tabs are **not** separate pages — they show/hide `data-page` sections
in one file. The toggle JS binds only to `[data-nav]` elements (~line 1717), so a
plain `<a href="...">` in the nav is a real link and won't be intercepted.

## Lessons

Each lesson is its own file (`lesson-<topic>.html`), never folded into `index.html`.
Copy `lesson-protection.html` and replace the `<section class="slide">` blocks; the
deck chrome, CSS, and money-input helpers carry over unchanged.

Lesson decks scale a fixed 1280×720 stage to fit. Below 860px they switch to a
stacked reading layout — scaling that far down makes text illegible. Sizing falls
back to a synchronous path when `document.hidden`, since rAF pauses in background tabs.

At three or more lessons, build a `lessons.html` hub and repoint the nav tab at it.
Once a third lesson exists, consider extracting shared `lesson.css` / `lesson.js`.

## Contact

Phone **(480) 589-2068** · talonrucker@gmail.com · linkedin.com/in/talonrucker
