# Website edit list

Source: `Prompts for Claude Pt.3.docx` (22 Jul 2026). 45 items — Batches 1–10, all done.
Source: `Prompts from Claude pt.4.docx` (23 Jul 2026). 20 items (#49–68) — Batches 11–18.
Batches 11–16 done; Batches 17–18 **outstanding**.

**How to use this:** start a new thread and say *"do Batch N from EDITS.md"*.
Read `CLAUDE.md` first — it has the section map and the publishing gotcha.
Tick items off as they land. Work one batch per thread; they're sized to fit.

Reference screenshots from the docx: `../Backups/edit-screenshots/`

---

## Batch 1 — Label & content sweep ✅ DONE

Cheap text edits, no logic. All in nav / hero / creds / books.

- [x] 1. Hero button "Visit the More page" → "Lessons", pointing at `lesson-protection.html`
- [x] 2. Lesson deck back-link: relabel to "Home", make it obvious and green
- [x] 3. "Let's talk" → call/text CTA with **(480) 589-2068**
- [x] 4. Remove "Provo, UT" from hero eyebrow and contact block
- [x] 5. Add "Avid skier" chip next to Eagle Scout / AI enthusiast
- [x] 6. Add "8,000+ hours volunteer work" chip next to Finance Society / WMA
- [x] 7. Nav: "More" → "Fun", "Tools & Demos" → "Tools"
- [x] 8. Delete `#experience` resume/job-history section from home page
- [x] 9. Phone number in the top email button, matching the one at page bottom
- [x] 10. Book list: add *Rivers West* and *How Will You Measure Your Life?*
       (rank HWYMYL "Great", Rivers West "Everything else")
- [x] 11. Reading list: move "Self-help & growth" above "On my list to read"
- [x] 12. "Productivity tutorials" → "Talon's Tips to Success"
- [x] 13. Monte Carlo: cap success rate at 99%, never 100%
- [x] 14. Delete "Realizing retirement: passive income" dropdown (Cash Flow)
- [x] 15. PDF guide: remove "Download PDF", promote "Open in new tab" as primary

---

## Batch 2 — Master income & age control ✅ DONE

- [x] 16. One income + age input placed directly **above** the "Your savings
      potential" calculator, that auto-populates every calculator on the site.
      - Editing a value **inside** an individual calculator must NOT feed back
        to the master or to other calculators — master is the only broadcaster.
      - Persist via `localStorage` so the lesson decks (separate files) pick it up.
      - Note: "wealth building calculator" in the docx always means
        **"Your savings potential"**.

      Built as `#mAge` / `#mInc` in a `.master` card. Broadcast targets:
      income → `wcInc`, `hlvInc`, `lcInc`, `lqInc`; age → `hlvAge`, plus the
      compound teacher's `years` slider (it derives its start age from
      retirement age minus years). Key `tr-master`. **Add new calculators to
      the `incomeIds` / `ageIds` lists** at the bottom of the main `<script>`.
      Lesson decks read the key on load via `applyMaster()`.

---

## Batch 3 — Wealth-building calculator overhaul ✅ DONE

Biggest single job. Keep the current default view exactly as-is; new features
live behind an "Advanced" toggle.

- [x] 17. Add "Advanced" tab that reveals the new features below
- [x] 18. Taxes — use **real bracket math**, not a flat rate (confirmed)
- [x] 19. Inflation slider — **the current one does nothing**; make it work
- [x] 20. Compound interest on invested savings only
- [x] 21. Hover a bar → show that year's number (add to the **default** view, not
      just Advanced — Talon considers this important)
- [x] 22. Lifetime spending total at the bottom — **must NOT compound**
      (it isn't invested)
- [x] 23. Year 1 / Year 10 / Year 20 income steps, as a collapsed drop-down under
      Year 1 income so it takes no space unless opened. Replaces the assumption
      of a flat 4% raise every year for a whole career.

Screenshot `image11.png` shows only *how another site handled tax* — do **not**
copy its debt or lifestyle handling.

**How it was built.** All inside the wealth-calculator IIFE (`#wcChart`).

- **Income steps (#23)** — built as user-defined steps, not fixed 1/10/20 slots.
  "Income changes at set years" is a collapsed `<details>` under Year-1 income;
  "+ Add a step" appends a `{y, amt, g}` row (year, income, raise), each
  removable. Year 1 is always the implicit base step, reading `wcInc`/`wcGrow`,
  so the master card keeps feeding it. `incomeAt(y)` picks the latest step
  with `y <= year` and compounds that step's raise from that step's year.
  First added step defaults to year 10, next to 20, seeded with the income the
  old curve would have reached.
- **Taxes (#18)** — 2025 federal brackets + standard deduction, single and MFJ,
  in the `TAX` table. **Bump these when 2026 figures are confirmed.**
  Federal only; no state or payroll tax. Advanced-only, off by default.
- **Inflation (#19)** — the slider did work, but only on the "Today's dollars"
  tab, so it looked dead in the default view. Now it also (a) indexes the tax
  brackets forward each year and (b) drives a permanent sub-line under Realized
  wealth showing the figure in the *other* tab's units. Always visibly live.
- **Hover (#21)** was already in the default view; the tooltip now also shows
  that year's gross income and after-tax income.
- **#22** is a `.wc-total` line under the legend — the final cumulative red
  value, which was already non-compounding.

Resolves the open question on #23: income **jumps** to the entered amount at
that year, no interpolation. Explicit steps make interpolation moot.

---

## Batch 4 — Simple new calculators ✅ DONE

Same build pattern, one thread.

- [x] 24. **Lump sum calculator** → Tools ▸ Cash Flow. Investment slider (cap $5M,
      default $250,000) + years slider (up to 47, so an 18-year-old reaches 65).
- [x] 25. **Tax bracket calculator** → Tools ▸ Tax & Debt
- [x] 26. **Passive income calculator** — $1M at 10% funds $100k/yr forever.
      Show that the money never runs out.
- [x] 27. **Death benefit income replacement** → Tools ▸ Protection. Same logic as
      #26 but with a $1M death benefit replacing a salary. Must mirror the
      version in the lesson deck.

Plus, requested alongside this batch: a **Today's dollars** toggle on the home-page
compound-interest ("Roth") teacher — 3% inflation, discounts the whole curve.

**How it was built.**

- **Shared bits.** The 2025 bracket table moved out of the wealth-calculator IIFE
  to script top level as `FED` / `fedTax(income, status, idx)` — one source of
  truth for the wealth calculator (which indexes brackets forward, `idx > 1`) and
  the new bracket calculator (`idx = 1`). **Bump `FED` when 2026 figures land.**
  Two new helpers in the Extra Tools IIFE: `lineChart()` (340×160 `.x-svg sm`)
  and `drawdown()` / `yearsToZero()`.
- **#24 lump sum** (`lsAmt`) — number box **and** slider, kept in sync both ways.
  Checkbox discounts at 3% for today's dollars. Sub-line shows the multiple.
- **#25 brackets** (`tbInc`) — the teaching point is marginal vs effective, so the
  output tile shows the **effective** rate big and the marginal rate in the
  sub-line. Band-by-band table underneath, bars scaled to dollars taxed in each
  band; unused bands greyed, the marginal band bolded. Verified against a hand
  calculation: single, $125k, standard deduction → $19,247, 15.4% effective.
- **#26 / #27 share `drawdown()`** — a fixed dollar withdrawal against a
  compounding balance, 40 years. Draw exactly the return and the line is flat
  forever (that's the "never runs out" proof); draw more and it bends to zero and
  turns red, and the sub-line names the year it dies. #27 is the same model with
  the benefit as principal and the salary as the withdrawal.
- **`dbInc` was added to `incomeIds`** in the master broadcast, so "income to
  replace" auto-fills from the master card. Side effect: the default view shows
  the visitor's *real* gap rather than the tidy $1M/$100k example — the example
  still leads in the "Why it matters" text.
- **Roth toggle** — `.ci-tab` classes, deliberately **not** `.wc-tab`: the wealth
  calculator binds `.wc-tab` with a global `querySelectorAll`. Implemented as a
  real-return switch (`growth()` returns `1.08/1.03` in today's mode), so the
  curve, gridlines, node labels, tooltip and totals all follow from one change.

No lesson-deck death-benefit calculator existed to mirror yet, so #27 is now the
reference version — **Lesson 02 (#42) should reuse this math**, not reinvent it.

---

## Batch 5 — Complex new calculators ✅ DONE

- [x] 28. **Lifetime Roth toggle.** Keep the existing one-time-contribution view as
      the default; add a toggle for "contribute this every year until retirement".
      Hold contributions **flat** for now, with a note that limits may rise.
- [x] 29. Add **Traditional IRA and 401(k)** options with their respective
      contribution limits.
- [x] 30. **Roth vs Traditional vs 401(k) demonstration** — small visual showing
      the difference between the three.
- [x] 31. **Home affordability calculator** → Tools ▸ Liquidity & Assets.
      Interest rate, average down payment, fees, and the opportunity cost of
      that down payment had it stayed invested. Include an "optimal scale" with
      *retire early* as one option. Plus a plain link to Redfin.
      **Redfin embed is not possible** — no public embed, and their terms forbid
      iframing. Link only. (Confirmed with Talon.)
- [x] 32. **Risk tolerance survey** — short and clickable, scoring into
      Conservative / Moderate / Growth / Aggressive Growth.
      **Must be written from scratch** — published surveys are copyrighted.

**How it was built.**

- **`FED` moved again — now above every calculator** (was between the compound
  teacher and the wealth calculator). The compound teacher needs bracket math at
  its *first render*, and `const` is not hoisted, so leaving `FED` below it threw
  a TDZ error. Three helpers now sit with it: `marginalRate()`, `retireRate()`
  (one rung up/down the bracket ladder) and `assumedRetireRate()`, which is the
  single source both the teacher and the comparison card read. **Still bump `FED`
  when 2026 figures land.**
- **#28 / #29** — two pill rows above the deposit slider in the compound teacher,
  classed `.ac-tab`, deliberately **not** `.ci-tab` or `.wc-tab` (both already
  bound by global `querySelectorAll`). Account limits live in the `ACCT` table
  (Roth/Traditional $7,000, 401(k) $23,500 — 2025, under 50). Picking an account
  re-caps the slider's `max` and clamps a now-illegal value. "Every year" switches
  `valAt()` from `P·gᵗ` to an annuity-due sum; contributions are held flat per the
  decision log.
- The chart's `yOf()` was **factor-based** (`(f-1)/(ff-1)`), which only works for a
  single deposit. It's now value-based off `S.finalV`. Identical output in
  one-time mode, correct in annual mode.
- Pre-tax accounts show an after-tax sub-line under the headline number. It reads
  `assumedRetireRate()`, so it can never disagree with the comparison card below.
  Changing that card's select nudges `#seed`'s `input` event to re-render the
  teacher, which exposes no handle of its own.
- **#30** — `.rvt` card below the compound shell, its **own IIFE** reading the
  teacher's sliders live, so there are no duplicate inputs. Compares on equal
  *gross* dollars: pre-tax accounts take the whole contribution, a Roth is funded
  after tax so the same paycheck buys less — but the limit then holds already-taxed
  dollars, which is the Roth's real edge. At equal tax rates the three land within
  a rounding error and the bars go flat; the verdict line says so explicitly, so
  it reads as the lesson rather than as a broken chart.
- **#31** (`hm*`) — standard amortisation with a guard for a 0% rate. Opportunity
  cost grows down payment + closing costs at 8%; a companion figure grows the
  house at 4% so the trade isn't presented one-sided. The "optimal scale" is a
  four-band gauge on housing as a share of gross income, with **under 20% =
  "Retire early"**. `hmInc` was added to `incomeIds`. Verified by hand: $450k at
  6.5%/30yr, 13% down → $2,475 P&I, $499k lifetime interest.
- **#32** (`rt*`) — six questions, 1–4 each, range 6–24, split evenly into the four
  bands. Scores only once **every** question is answered, so a half-finished
  survey can't mislabel anyone. Original wording throughout.
- New shared `.bscale` / `.bband` four-band scale, used by both #31 and #32.
  Colour comes from a `k-*` class per band, since the two tools ramp differently.

---

## Batch 6 — Protection section & scorecards ✅ DONE

- [x] 33. Rebuild "What's your level of protection" to match `image13/14.png`.
      Ignore the warning pop-up in the screenshot. Make **Protection Overview
      Scorecard** the top drop-down.
- [x] 34. Term vs whole life graphs render distorted on web and mobile — fix or
      rebuild from scratch.
- [x] 35. Move every scorecard to the **top** of its section.
- [x] 36. Move "Savings & wealth building scorecard" into **Cash Flow**.
- [x] 37. New scorecards for **Retirement, Tax, and Liquidity**.
- [x] 38. Port the life insurance qualification + disability gap calculators from
      `lesson-protection.html` into Tools ▸ Protection. Add a field for
      **current disability insurance** (see `image3.png`).

**How it was built.**

- **Chart sizing — the root cause of #34.** `.x-svg` is `width:100%` over a small
  viewBox, so a full-width container stretched a 340×170 chart to ~1080×540 and
  scaled every stroke and label with it. Batch 4 had capped only its own charts
  (`.x-svg.sm`, 560px). The cap is now on **`.x-svg` itself** (640px, centred),
  so the savings-gap and Monte Carlo charts stop rendering half a screen tall.
  `.tw-card svg` is capped at 420px. **Any new chart inherits the cap**—use
  `.x-svg` and don't set a width.
- **#34 charts rebuilt** on a 300×180 viewBox (was 220×130) to match `image14`:
  grey plot rect, `$` on the axis, `YEARS` centred beneath, labels placed clear
  of the curves rather than sitting on them. Term = level benefit with an end
  tick + a cost curve that bends up late; whole life = drifting benefit, level
  cost, cash value compounding past both.
- **#33** — inputs are now four side-by-side `.pcard` boxes (Life / Disability /
  Liability / Wills & Trusts), 4→2→1 columns as the screen narrows. The old
  `lpWills` three-way select is gone; Wills and Trusts are **separate yes/no
  radios** (`lpWill*` / `lpTrust*`) as in the screenshot. Gap logic is unchanged:
  a will with no trust still reads as one gap, not zero. The warning pop-up in
  the screenshot was deliberately not built.
- **#35–37** — every section now leads with its scorecard. Savings & wealth
  building moved out of Liquidity into Cash Flow (#36) and gained two rows
  (employer match, automatic saving). Three new scorecards: Liquidity, Tax &
  Debt, Retirement readiness. All use the existing `table[data-scard]` markup,
  so the one click handler picks them up with no JS change.
- **#38** — both calculators ported to the site's `.xc-grid` idiom (number
  inputs, not the deck's money-text helpers). The age→multiple `rule()` is
  **duplicated** from `lesson-protection.html`; if the guideline table changes,
  change it in both. `liInc` / `diInc` added to `incomeIds` and `liAge` to
  `ageIds`, so the master card feeds them. The DI tool covers `image3.png`'s
  three lines (monthly income, current DI benefit, gap) in a `.ldg` ledger, plus
  the cap and essential-expenses logic from the deck.

---

## Batch 7 — Balance sheet pop-ups ✅ DONE

- [x] 39. Make the balance sheet (`#balancesheet`, ~line 1022) interactive —
      click a line to open a pop-up and enter numbers.
      Layout ideas in `image4–8.png`; treat as inspiration, not spec.

**How it was built.**

- Every line is now a `<button data-bs="key">`; the totals are `<b data-bs-out>`.
  **The `<b>` text is written by JS on every render — don't hard-code numbers
  into the markup, they'll be overwritten.**
- **One shared pop-up** (`#bsModal`), not a drawer per band. The screenshots show
  inline drawers, but four of them at four different widths is a lot of mobile
  layout risk for no gain — and Talon asked for pop-ups. Its fields are built
  from the `GROUPS` table, which is the only place to touch when adding a line:
  give the group a `t` (title), `tone` (`protect`/`assets`/`liab`/`cash`, which
  picks the header colour) and an `f` list of `M(key,label)` money or
  `Y(key,label)` yes/no fields. Seed the key in `DEF` too.
- **State is a flat bag of numbers** (`S`), so totals are plain sums and the whole
  thing persists as one `localStorage` blob under `tr-bsheet`. `DEF` doubles as
  the illustrative example and the "Reset to example" target.
- The sub-lines the screenshots break out are all here: savings splits into cash +
  cash-value life, retirement into pre-tax + Roth, short term into cards/auto/
  student, protection costs into six premiums, debts & taxes into tax + payments.
  Wills is four yes/no radios (will, trust, POA, living will) shown as "3 of 4".
- **Compact formatting (`$1.3M`) is protection-band only.** The cash band sits
  beside dollar totals and `$8,000` next to `$40K` reads as a bug.
- Earnings is fed by the master card — but the pop-up's inputs don't exist in the
  DOM until it opens, so `incomeIds` can't reach them. This IIFE reads `tr-master`
  itself at init and listens to `#mInc`. Still one-way; typing in the sheet never
  moves the master.
- Money inputs are `type="text"` + `inputmode="numeric"`, parsed on input and
  re-formatted with commas on blur, so `$420,000` reads as it does in the sheet.

---

## Batch 8 — Cash flow section rework ✅ DONE

- [x] 40. "Interrupt the spending circuit": show the original
      *income → checking → lifestyle* flow first, then progress to
      *income → wealth building → checking → lifestyle*.
- [x] 41. Lifestyle-creep visual renders badly — rebuild it. Whatever design lands
      in the Batch 9 cash-flow lesson should be reused here (and in
      "engineering an increased savings rate").

**How it was built.**

- **#40 — one interactive `.scircuit` widget**, used in both places the section
  appears (home Tools drawer and Page 3 ▸ Cash Flow). A two-tab segmented control:
  "The typical setup" shows *Income → Checking → Lifestyle*; "Interrupt it" splices
  a **Wealth-Building Account** node (with its allocation pills) between Income and
  Checking, animating in via a `max-height`/`opacity` transition. The caption swaps
  per state. The old side-by-side `.cf-grid` (home) and single `.flow` (tools) and
  their CSS are gone — one component replaces both, so the diagram can never drift
  between pages. JS is one IIFE over every `[data-scircuit]`; add a node to the
  markup and it just works.
- **#41 — one shared `creepChart(svg, opts)`** replaces the crude hand-drawn
  three-`<path>` SVG. It draws a clean computed chart with a shaded region, curve
  labels with a white halo (so text stays legible over lines), and YEAR 1 / YEAR N
  axis labels. Two modes from one function:
  - *Engineering an increasing savings rate* — `gapTo` mode: income @4%, expenses
    @4% (red dashed), expenses held @3% (orange), with the widening
    **wealth-building room** shaded between income and the constrained line.
    Matches `image12` (top). Illustrative, drawn once.
  - *1% lifestyle-creep test* — `area` mode: the redirected savings compounding to
    a green **wealth badge** at the endpoint, tied live to the calculator (badge =
    the "Extra wealth" headline, so they can't disagree). Matches `image12` (bottom).
    The test previously had no chart at all.
  `creepChart` carries its own SVG namespace constant, so it's safe to call from the
  earlier-positioned creep-test IIFE (no TDZ on the shared `NS3`). **Reuse this same
  function in the Batch 9 cash-flow lesson** rather than reinventing the chart.

---

## Batch 9 — Lessons 02 & 03 ✅ DONE

Separate files — these never touch `index.html`, so this is the cheapest batch.
(Nav repoint is the one exception — see below.)

- [x] 42. **Lesson 02 — Cash flow.** `lesson-cashflow.html`, 8 slides. Wealth-building
      account (interrupt-the-circuit toggle), lifestyle creep (`image12`), the 1% test,
      401(k) match & work benefits, what a rollover is, passive income (#26 model).
- [x] 43. **Lesson 03 — Investing & risk.** `lesson-investing.html`, 7 slides. Freeway
      analogy with animated lanes, lane-matcher, the crash, and buy-low/sell-high.
- [x] 44. **Buy low / sell high** — folded into Lesson 03 (slides 5–6): the emotion
      cycle (euphoria = feels-like-buying / panic = actual-opportunity) plus a
      two-investor interactive.

**How it was built.**

- Both decks are **self-contained copies** of `lesson-protection.html` — same chrome,
  CSS, 1280×720 scale-to-fit, money-input helpers, mobile stacked fallback, and
  `applyMaster()` reading `tr-master`. CLAUDE.md's "consider extracting shared
  lesson.css / lesson.js" was **not** done: the primary instruction is to copy, and
  refactoring a working deck to external files added risk for no user-visible gain.
  If a 4th lesson lands, revisit extraction then.
- **Lesson 02.** The spending-circuit toggle mirrors the site's `.scircuit` concept
  (Batch 8 #40) rebuilt in deck style. The lifestyle-creep chart mirrors `creepChart`'s
  gap-wedge design (Batch 8 #41) — hand-drawn here rather than imported, since decks
  can't share the site's JS. The **1% test** calculator invests the gap between
  spending-at-income-rate and spending-held-lower and compounds it; verified live
  (income auto-filled from master → $5M from the gap over 30 yrs). **Passive income**
  reuses the #26 `drawdown()` / `yearsToZero()` model verbatim: $1M at 10% withdrawing
  $100k stays flat forever; push the draw past earnings and it names the year it dies.
- **Lesson 03.** Freeway is CSS lanes with animated `.car` cars (reduced-motion guard)
  and an HOV diamond. Lane-matcher maps horizon → lane + a stock/bond/cash mix bar.
  The crash and emotion-cycle charts are drawn SVGs. The **two-investor** interactive
  runs one market path (crash at year 4, recovers to +40% regardless of depth) three
  ways — Steady rides through (+40%), Panicker locks the bottom loss, Opportunist adds
  $5k fresh cash at the low. Verified: 40% drop → Steady $14k / Panicker $6k / Opportunist
  $25,667; deeper crash only hurts the Panicker, never the Steady.
- **`lessons.html` hub** built because this is the **third** lesson (CLAUDE.md rule).
  Three cards, site design language, links to each deck; nav "Lessons" tab and the hero
  "Lessons" button now point at the hub (changed in **both** index files, still `cmp`
  identical). Line ~1323's "Same math as the Protection lesson" deep-link was left
  pointing at `lesson-protection.html` — it's a specific reference, not navigation.
- **All three decks now return to the hub** ("← All lessons" → `lessons.html`).
  Lesson 01's back-link was the Batch 1 "Home → index.html" button; with a hub now
  in place, the hub is the lessons' home and the hub's own "Back to site" returns to
  index. See decisions log.

---

## Batch 10 — Fun page & tutorial ✅ DONE

- [x] 45. **Site tutorial — spotlight the tabs.** DECIDED 22 Jul 2026.
      - Delete the `#explore` "Jump straight to what matters" section outright
        (confirmed via `image9.png`). Let the page close up — shorter is better.
      - Tour trigger goes in the **hero**, next to the Lessons button.
      - On click: dim the page and spotlight each nav tab / major section in
        turn, with a short label.
      - Give every label a **one-line "why it matters"** alongside the name.
        Talon's goal is teaching concepts, not just showing where to click, and
        the label costs nothing extra. Keep to one sentence — fewest words wins.
      - Visitor-paced (Next / Skip), never auto-advancing. Remember completion
        in `localStorage` so it doesn't nag returning visitors. Keep the trigger
        on the page so the tour can be replayed.
      - Must work on mobile, where the nav tabs are hidden below 860px —
        check what the spotlight targets there before building.

      Note: in the source docx, screenshots appear *above* the text that
      refers to them. `image10.png` belongs to the wealth calculator tax
      request in Batch 3, not to this item.
- [x] 46. Easter egg: a $10 bill on a fishing hook that swims across the screen.
      Clicking it offers to help open a Roth IRA, with Talon contributing $10.
      **Fun page only.**
- [x] 47. Red "DO NOT PRESS" button → full-screen confetti celebration.
- [x] 48. Favourite books: let visitors suggest additions.
      **A static site can't accept submissions** — resolved as text/email
      (confirmed with Talon).

**How it was built.**

- **#45 spotlight tour.** `#explore` deleted outright (its `.link-card`/`.link-grid`
  CSS is now dead but left in place — harmless, and removing it risked the shared
  media-query blocks). Trigger is a third hero button, `#tourBtn` "☼ Take the tour",
  beside Lessons. The tour is **one IIFE** at the end of `<body>`: a transparent
  full-screen `.tour-back` catches clicks, a `.tour-spot` rectangle does the dimming
  via a huge `box-shadow` with a "hole" over the target (pointer-events none, so the
  spotlighted tab can't be clicked mid-tour), and a `.tour-pop` card shows the step
  counter, name, one-line why, Skip / Next. Steps are a **selector-driven `STEPS`
  array** — missing targets are skipped, so it degrades gracefully. Five stops: the
  four nav tabs (Home/Tools/Fun/Lessons, visible at every width — the "tabs hidden
  below 860px" note was stale, they only shrink) plus `.comp-head` (the compound
  teacher, a compact heading so a section taller than the viewport never pushes the
  pop off-screen). `place()` flips the pop above the target when there's no room below
  and clamps it inside the viewport both ways. Completion (Done **or** Skip **or** Esc)
  writes `tr-tour-done`; first-time visitors get a `.pulse` on the trigger, **never an
  auto-launch** — the pulse is the only "nag" and it clears once seen. Reduced-motion
  guard on the spotlight transition and pulse. **Add a stop = add one row to `STEPS`.**
- **#46 fishing egg.** Fun-page-only: a fixed `.fish-egg` button ($10 CSS banknote on a
  🪝 hook with a swaying line) drifts across the viewport at a random depth via a rAF
  tween, re-scheduling itself after each pass. A `MutationObserver` on `#top`'s
  `data-active` starts it only while Fun is active and hides it elsewhere. Clicking
  opens the `#eggModal` "You caught it!" card — Talon chips in $10 toward a first Roth,
  with the Fidelity 10-min walkthrough (the same YouTube link the deleted `#explore`
  used) and a pre-filled `mailto` to claim it. Reduced-motion stops the bob.
- **#47 DO NOT PRESS.** Its own centred Fun-page card with a red `.dnp-btn`. Click fires
  a **self-contained canvas confetti burst** (`#confetti`, no library): ~160 rects
  (70 under reduced-motion) in the site palette, gravity + spin, fading out, and the
  canvas hides itself once the last particle clears. rAF-driven, pointer-events none.
- **#48 book suggestions.** No backend, so a one-line `.book-suggest` invite under the
  reading list: "Text it" (`sms:` prefilled) or "email it" (`mailto:` prefilled). Text
  only, per the decision — matches the "Not doing: visitor-editable book list" note.

---

# Pt.4 — outstanding (23 Jul 2026)

New source doc, 20 items. Screenshots: `../Backups/edit-screenshots-pt4/`
(`tax-color-chart.png` → #50, `lifestyle-creep-gap.png` → #56). Same rules apply —
read `CLAUDE.md` first, one batch per thread, edit **both** index files.

---

## Batch 11 — Home calculators: limits, master, inflation ✅ DONE

The three big home-page calculator jobs. All touch the compound/Roth teacher, the
"Your savings potential" calculator, and the master start-here card — done in one
thread so the shared inflation / today's-dollars plumbing stays consistent.

- [x] 49. **2026 contribution limits + "your age" input** (Roth teacher).
      - Roth & Traditional one-time-deposit slider → cap **$7,500**; 401(k) → **$24,500**.
      - Confirm the "today's dollars" math is right at **3%/yr** inflation.
      - Rename the **"from age"** control to **"your age"** and make it a typeable
        number that auto-drives the "years to grow" slider (e.g. type 24 → 41 years
        to reach retirement age 65). Visitors mistook "years to grow" for their age.
- [x] 50. **Wealth calculator: drop the inflation slider, fix the math.**
      - Remove the inflation **slider** but keep the **"Today's dollars"** button
        (currently flaky — sometimes works, sometimes not). Default 3%, verify correct.
      - In Advanced, make the **taxes bar change colour** like `tax-color-chart.png`.
- [x] 51. **Master start-here + savings-potential fixes.**
      - Changing age in the start-here section must also re-run **"Your savings
        potential"** (it doesn't today).
      - Confirm dialog when editing master age/income: "Are you sure? This changes
        every calculator." (yes / no).
      - On first load, default **after-tax return = 0** and **lifestyle spend = 0**,
        so realized wealth equals the "your paycheck is your biggest asset" figure
        (only years, income, raise% in play). Fix the calc so this actually holds.

**How it was built.**

- **#49 limits** — the `ACCT` table (compound teacher IIFE) went Roth/Trad $7,000→
  **$7,500** and 401(k) $23,500→**$24,500**; the seed slider's HTML `max` bumped to
  7500 (Roth is the default account, so it sets the initial ceiling). The two "2025
  limits" notes now read 2026. **Contribution limits are 2026; the `FED` bracket
  table is still 2025** — they're separate, bump each when its figures land.
- **#49 today's dollars** — verified correct: `growth()` returns `(1+.08)/(1+.03)`,
  the standard real-return discount at a fixed `INFL=0.03`. No change needed.
- **#49 "your age"** — the old "from age X" was a *display span* under a Years-to-grow
  slider, which visitors read as their age. Added a typeable **`#ciAge`** "Your age"
  field. Age is the anchor: `years = retire − age`, fully bidirectional — typing an
  age drives the years slider, moving the slider writes the age back, changing
  retirement age keeps the age fixed and moves years. Values clamp to the slider's
  5–47 and normalise on blur. The `startNote` span was repurposed to show **"to age
  65"** (the retirement target). The master card still drives the *years* slider, so
  its age flows straight into the mirrored `#ciAge`.
- **#50 inflation slider** — removed outright; inflation is now the fixed `INFL=0.03`
  constant. **This is the "flaky Today's dollars" fix**: when a visitor dragged the
  old slider to 0%, today's = future dollars and the toggle looked dead. At a fixed
  3% it always visibly changes the number. Verified: future $18.1M ↔ today $5.56M,
  stable across repeated toggles, sub-lines cross-referencing correctly.
- **#50 tax bar colour** — matches `tax-color-chart.png`: the downward bars now
  **stack lifestyle (brick red, at the zero line) then taxes (dark maroon `#5e1f16`,
  below it)**. `compute()` tracks cumulative `taxCum`; `maxDn` scales on `red+taxCum`;
  a `.wc-bar-tax` rect draws under each red one. A **Taxes** legend entry and a
  tooltip "Taxes paid" line appear only when Advanced taxes are on (0-height/hidden
  otherwise, so the default view is unchanged). Verified the tax rect sits exactly
  at `redY + redHeight`.
- **#51 age re-runs savings potential** — `wcYears` (study period) was never fed by
  the master. Added `fire(wcYears, 65−age)` to the master's `apply()`, so age now
  drives the savings-potential horizon the same way it drives the compound teacher.
- **#51 confirm dialog** — the master now broadcasts on **`change` (commit), not per
  keystroke**: a native confirm ("Are you sure? This changes every calculator on the
  page.") gates each committed edit; **No** reverts the field to its last confirmed
  value and re-syncs live listeners (the balance sheet reads `#mInc` on input). Load
  and cross-tab `storage` syncs call `apply(false)` and skip the confirm.
- **#51 zero defaults** — HTML defaults are now **after-tax return 0%, lifestyle 0%
  (save 100%)**. With `r=0, lifeR=0` the calc already yields `realized = Σ net income`
  = total career earnings, i.e. the "your paycheck is your biggest asset" figure
  (only years, income, raise% in play). Verified: age 25, $250k @4% over 40 yrs →
  **$23.76M** realized, matching the closed-form sum.

---

## Batch 12 — Account explainer & compliance ✅ DONE

- [x] 52. **Replace the "same money, three accounts" demo.** Delete it from beneath
      the Roth calculator on the home page. Build a clearer version under **Cash Flow**
      (Tools) explaining: Roth = tax **now**, Traditional = tax **later**, 401(k) =
      work restrictions & contribution limits. Design is open — pick the clearest form.
- [x] 53. **Compliance disclaimer pop-up.** On a visitor's **first** calculator click,
      show a pop-up with an **Accept** button: "these calculators are for conceptual
      purposes only, not financial-planning advice." Confirm whether a footer line is
      also needed for compliance (context: future Series 7 / 66 + broker-dealer).

**How it was built.**

- **#52** — the old `.rvt` "Same money. Three accounts." bar chart (Talon found it
  confusing) is **deleted from the home page** and replaced by a plain three-card
  comparison under **Tools ▸ Cash Flow** (`.acct3`, after "Where does your income
  actually go?"). Each card leads with a **tax-timing pill** — Roth *Taxed now*,
  Traditional / 401(k) *Taxed later* — then the 2026 limit ($7,500 / $7,500 /
  $24,500) and a one-line "best when." A "your retirement tax rate" select drives a
  verdict and highlights the winning card(s): lower → Traditional + 401(k), higher →
  Roth, same → Roth edges it. **Cheaper than the old bar demo and reads at a glance.**
- **The select kept its `#rvtRate` id on purpose.** `assumedRetireRate()` (and the
  compound teacher's after-tax sub-line) read it; it now lives on Page 3 but is still
  in the DOM, so the after-tax line still updates when it changes — verified live
  across pages (32% → 35% retirement rate flows through). The `.rvt` CSS is now dead
  but left in place, matching the project's practice for retired components.
- **#53 first-use pop-up** — `#discModal` reuses the `.egg-modal` styles. A document-
  level capturing listener watches for the first **trusted** `input`/`change` on any
  form control (or click on a scorecard / balance-sheet / gauge). **`isTrusted` is the
  key**: the master card's programmatic broadcast on load fires synthetic events, and
  the guard makes those a no-op so the pop-up never shows itself on load (verified).
  Accept (or Esc) records `tr-disclaimer-ok` in `localStorage`, so it shows **once**.
- **#53 footer** — resolved the open question by **adding both** (safe default). The
  footer disclosure now explicitly covers "the calculators and figures … are
  hypothetical illustrations for conceptual and educational purposes only — not
  investment or financial-planning advice," alongside the first-use pop-up.

---

## Batch 13 — Cash flow: spending circuit & lifestyle creep ✅ DONE

- [x] 54. **Interrupt the spending circuit → side by side (home).** Currently the
      better solution is hidden behind a click. Show the typical setup and the
      interrupted version **side by side**, with a reveal button next to the typical
      one so the fix appears once they understand the default.
- [x] 55. **Match it on the Tools page** — the tools-page spending circuit should look
      and behave exactly like the reworked home-page one (#54).
- [x] 56. **Merge the two lifestyle visuals.** Combine the **"engineering an increased
      savings rate"** drop-down into the **1% lifestyle-creep test** (they teach the
      same thing). Keep the creep test's big gap visual (see `lifestyle-creep-gap.png`):
      raise expenses 3% on a 4% raise → large lifetime difference.

**How it was built.**

- **#54 / #55 — one new `.scircuit2` component** replaces the old tabbed `.scircuit`
  in **both** places (home Cash-flow drawer + Tools ▸ Cash Flow), so the two can never
  drift. Two columns: the **typical setup** (Income → Checking → Lifestyle) always
  visible on the left; the right column shows a "**Show the fix →**" button until
  clicked, then reveals the **interrupted** flow (Income → Wealth-Building Account →
  Checking → Lifestyle). The reveal is one-way and un-remembered — the point is to see
  the default *first*, then the fix beside it. Reuses the existing `.sc-node` /
  `.sc-arrow` / `.sc-allocs` styling; collapses to one column below 640px. The old
  `.sc-tab` CSS is now dead but left in place. JS is one IIFE over `[data-sc2]`.
- **#56 — merged into the 1% test, dropped the "Engineering" card.** The two taught the
  same lesson, so "Engineering an increasing savings rate" (a static `engChart`) is
  gone and its idea folded into **"The 1% lifestyle-creep test."** The test now models
  the **gap**: income rises at your raise (default 4%), lifestyle rises slower by the
  amount you hold back (default 1% → 3%), and each year's widening gap is invested and
  compounds. The chart is the **big gap visual** from `lifestyle-creep-gap.png` (green
  income line, orange lifestyle line, shaded "wealth-building room," future-wealth
  badge) via `creepChart`'s `gapTo` mode — replacing the old accumulation area chart.
  New sliders: raise % and "hold lifestyle below your raise" (the 1%). Verified
  monotonic: 0.5% → $2.23M, 1% → $4.28M, 4% → $13.50M at $200k / 30 yrs / 6%.
  `lcInc` kept its id, so the master card still feeds income.

---

## Batch 14 — Protection section ✅ DONE

Two of these are questions Talon wants answered, not just edits (#57, #59).

- [x] 57. **Explain the outstanding-debt field** in "how much life insurance would you
      qualify for" — Talon doesn't see how it applies. Answer first; keep, reword, or
      remove based on the answer.
- [x] 58. **Rework "what a death benefit replaces."** The 40-year chart swings too hard
      on tiny input changes. Reframe around the core idea: $1M benefit at 10% return
      throws off $100k/yr to cover expenses **indefinitely**. Use a short horizon
      (~3 years), maybe a click-through to the next section. Apply the **same
      treatment** to "the day your money works instead of you" — change both together.
- [x] 59. **Term vs whole life: cap cash value at the death benefit.** The graph shows
      cash value growing **above** the death benefit — confirm that's wrong and fix it.
- [x] 60. **Move the monthly disability-gap calculator** next to the other disability
      drop-downs.

**How it was built.**

- **#57 answered: kept.** Outstanding debt *is* a legitimate underwriting input — financial
  underwriting lets you insure enough to **both** replace income and clear debts the family
  would inherit (mortgage, loans), so it's added on top of the age×income multiple. The tool
  already did this math (`max = base + debt`); the confusion was that nothing explained *why*.
  Fix is copy, not code: a one-line hint under the debt field ("Mortgage and loans your family
  would inherit — coverage can clear these on top of replacing income") and a reworded
  guideline note. Field stays.
- **#58 reframed both tools.** The 40-year `lineChart` auto-scaled to a peak or crash, so tiny
  input changes swung the whole curve. New shared **`foreverChart`** (script top level, beside
  `drawdown`) draws a **3-year** view: a blue balance ribbon anchored to the starting balance
  (flat when you draw exactly the return) over a few equal green income bars trailing into "…".
  Draw the return → ribbon flat + "…" = the paycheck reads *indefinite*; over-draw → ribbon
  tilts down, bars turn red, "runs out". Both `dbChart` (death benefit) and `piChart` ("the day
  your money works instead of you") now call it, and their sub-lines/notes say "indefinitely"
  instead of "after 40 years". The full 40-yr `drawdown`/`yearsToZero` is still computed for the
  "runs out in year N" wording. No click-through added — the short-horizon reframe carries it.
- **#59 confirmed wrong, fixed.** Real whole-life cash value grows *toward* the death benefit
  and converges to it at maturity — it never exceeds it. The SVG's blue cash-value curve ended
  at y=26, above the yellow death-benefit curve's y=44 (lower y = higher value). Redrew the
  cash-value path to end at y=49, just below the benefit, staying under it the whole way. Bullet
  now reads "climbs toward the death benefit, never above it."
- **#60** — the "Your monthly disability gap" calculator moved from slot 3 (right after the
  life-insurance qualifier) to sit **directly above "Disability: group vs. individual"**, so all
  three disability pieces (gap calc, group-vs-individual, two-contracts) are now adjacent. Pure
  block move; no id or JS change.

---

## Batch 15 — Scorecards, liquidity & housing ✅ DONE

- [x] 61. **Savings & wealth-building scorecard** relabels/reorder:
      - Put "credit saving is automatic" directly under "wealth building account".
      - "wealth building account target funding" → **"investments selected"**.
      - "lifestyle costs mapped" → **"fixed expenses mapped"**, and **add** a separate
        "lifestyle expenses mapped" row.
- [x] 62. **Liquidity scorecard:** rename "sinking funds for known costs" →
      **"prepared funding for known costs"**; add a **stabilized-income** row (for
      volatile/commission earners — steady cash through the year vs feast-or-famine;
      name it as you see fit).
- [x] 63. **Life-event funds side by side** — most liquid on the **left**, least liquid
      on the **right**.
- [x] 64. **"What home can you afford"** — rename "how much house can you actually carry"
      to something that rings better. Add a **Predict** button next to the tax /
      insurance / HOA monthly field that estimates it from home price, down payment,
      interest rate and income — but keep the manual override.

**How it was built.**

- **#61** — the scorecard is `table[data-scard]` markup, so this was rows only. New order/labels:
  Savings rate · Wealth-building account · **Saving is automatic** (moved up, directly under the
  account row) · **Investments selected** (was "WBA target funding") · Employer match captured ·
  **Fixed expenses mapped** (was "Lifestyle costs mapped") · **Lifestyle expenses mapped** (new).
  Each row keeps its existing default mark; the new row defaults to Moderate. No JS change — the
  one click handler picks up rows generically.
- **#62** — "Sinking funds for known costs" → **"Prepared funding for known costs"**, and a new
  **"Uneven income smoothed across the year"** row (the stabilized-income check for
  commission/volatile earners), defaulting to Warning like its neighbour. Markup only.
- **#63** — the life-event funds tool stacked Most-liquid then Least-liquid in one column. Now a
  new **`.lqcols`** 2-col grid puts **Most liquid on the left, Least liquid on the right**, with
  Annual income full-width beneath and the result panel still on the right of `.xc-grid`. All
  eight `lq*` ids unchanged, so the `lqMost`/`lqLeast` JS needs no edit. `.lqcols` collapses to
  one column under 720px (added to the existing media query).
- **#64** — heading "How much house can you actually carry?" → **"What home can you actually
  afford?"** A **Predict** button under the "Tax, insurance & HOA / mo" field estimates that
  figure from the home price and down payment: property tax ~1.1%/yr + homeowners insurance
  ~0.35%/yr + **PMI ~0.6%/yr of the loan when under 20% down**, ÷12, rounded to $25. It writes
  into `hmFees` and re-runs, so the visitor can still **override** by typing. The bottom note now
  says Predict includes a PMI estimate under 20% down (previously the tool excluded PMI).

---

## Batch 16 — Fun page polish ✅ DONE

- [x] 65. Make the floating fish hook **a little bigger**.
- [x] 66. **"DO NOT PRESS" upgrade** — bigger payoff: fireworks + confetti lasting a few
      seconds, with **4 different random effects** it cycles through per click.

**How it was built.**

- **#65** — pure CSS in the `.fish-egg` block. The 🪝 hook (`.fish-egg` font-size)
  went **2.1rem → 2.7rem**, with the line (34→42px) and the $10 bill (.8→.92rem)
  scaled to match so the bait stays in proportion. Both index files.
- **#66** — the old single centre-burst `burst()` became a **four-payoff system the
  button cycles through** (`fxIdx` advances one step per press, so repeated presses
  reliably show all four): **fxCannon** (central confetti explosion), **fxFireworks**
  (7 staggered shells over ~1.75s, each a radial ring of glowing round *sparks* —
  new `shape:1` particle drawn as a shadow-blurred `arc`), **fxRain** (6 waves of
  confetti falling the full width over ~1.3s) and **fxSides** (two bottom-corner
  cannons firing inward). Staggered waves use a `later()` helper that tracks a
  `pending` count, so `tick()` keeps the canvas alive while `parts.length || pending>0`
  — that plus ~1.5–3s particle lifetimes makes each payoff **last a few seconds**.
  Still library-free canvas; reduced-motion cuts the counts/waves but keeps all four.
  New bright `SPARK` palette for the fireworks; the confetti keeps the site `COLORS`.

---

## Batch 17 — Fun page: Flappy-Talon game (big, own thread)

- [ ] 67. **Flappy Bird clone** on the Fun page: an **eagle with large talons** (Talon's
      name — the joke). Dollar bills instead of coins, pipe-style obstacles, a
      **fullscreen** toggle, and a **score**. Bonus idea: collect fifteen $15 bills →
      offer $15 to start a Roth IRA (tie into the existing Roth-egg flow).

---

## Batch 18 — Lesson 03: HOV lane

- [ ] 68. In the **investing lesson's HOV-lane analogy**, make the **HOV lane slightly
      faster than the far-left lane**, and make it **work on mobile** (only the HOV
      lane is broken on mobile; the others work).

---

## Open questions

- **#53** — ~~Does compliance need a persistent footer disclaimer in addition to the
  first-click pop-up?~~ **RESOLVED (safe default): built both** — first-use pop-up +
  a footer line explicitly covering the calculators. Talon can pare back if counsel
  says only one is needed.
- **#57** — ~~Awaiting Talon's read on whether outstanding debt stays in the life-
  insurance qualifier once explained.~~ **RESOLVED: kept.** Debt is a real financial-
  underwriting input (coverage can clear inherited debt on top of replacing income); the
  field stays, with a hint that now explains why. Talon can still ask to remove it.
- **#67** — Confirm the "$15 bills → $15 Roth" reward mechanic vs. just a high-score.

## Decisions log

- **23 Jul 2026** — Life-insurance qualifier keeps the **outstanding-debt** field: it's a
  legitimate underwriting input (insure enough to replace income **and** clear inherited
  debt). The fix was an explanatory hint, not removal. (#57)
- **23 Jul 2026** — Death-benefit and passive-income charts reframed from a 40-year balance
  line (which swung on tiny input changes) to a shared **`foreverChart`** 3-year view: a flat
  balance ribbon over equal income bars trailing into "…", so drawing the return reads as
  *indefinite* income. Both tools share it so they can't drift. (#58)
- **23 Jul 2026** — Whole-life **cash value capped at the death benefit** — it converges toward
  the benefit and never exceeds it. The old SVG drew it crossing above; confirmed wrong, redrawn
  to end just below. (#59)
- **23 Jul 2026** — Home-affordability **Predict** button estimates tax + insurance (+ PMI under
  20% down) from the home price and down payment as a starting figure the visitor can override —
  it does not lock the field. (#64)
- **22 Jul 2026** — Wealth calculator income steps are **user-defined and
  unlimited** (add/remove a step at any year), not fixed Year 1/10/20 slots.
  Income jumps at each step, then grows at that step's own raise. (#23)
- **22 Jul 2026** — Tax table uses 2025 federal brackets, indexed forward by the
  inflation slider. Swap in 2026 figures once confirmed. (#18)

- **22 Jul 2026** — Tutorial: spotlight-the-tabs, not a guided scroll tour.
  Explore section deleted, trigger moves to the hero. (#45)
- **22 Jul 2026** — Wealth calculator taxes use real bracket math, not a flat rate.
- **22 Jul 2026** — Roth contributions held flat, with a note that limits may rise.
  Traditional and 401(k) added alongside, with their own limits.
- **22 Jul 2026** — Redfin embed dropped; plain link instead.
- **22 Jul 2026** — Book suggestions handled by text/email, not a form.
- **22 Jul 2026** — Risk tolerance survey written from scratch (copyright).
- **22 Jul 2026** — Federal bracket table hoisted to one shared `FED` / `fedTax()`
  at script top level, used by both the wealth calculator and the new bracket
  calculator. Update it in one place. (#18, #25)
- **22 Jul 2026** — Passive income (#26) and death benefit (#27) share one
  `drawdown()` model, so the two tools can never drift apart.
- **22 Jul 2026** — Roth vs pre-tax is compared on equal **gross** dollars, and
  the retirement tax rate is one rung up/down the visitor's own bracket rather
  than a number they never gave. The compound teacher's after-tax line and the
  comparison card share `assumedRetireRate()` so they can't disagree. (#28–30)
- **22 Jul 2026** — Every `.x-svg` chart is capped at 640px wide (560px for
  `.sm`). Uncapped, a small viewBox stretched to full container width rendered
  ~540px tall with distorted strokes — the real cause of #34. (#34)
- **22 Jul 2026** — Wills and Trusts are two separate yes/no radios, replacing
  the old three-way select, per `image13.png`. The warning pop-up in that
  screenshot was deliberately not built. (#33)
- **22 Jul 2026** — The life-insurance `rule()` table is duplicated between
  `index.html` and `lesson-protection.html`. Two files, one rule — update both
  together, or extract it if a third copy ever appears. (#38)
- **22 Jul 2026** — Balance sheet uses one shared pop-up rather than the per-band
  inline drawers in `image4–8.png`. Same interaction, far less mobile layout risk,
  and it matches what Talon actually asked for ("pop-ups"). (#39)
- **22 Jul 2026** — Balance sheet numbers save to `localStorage` on this device
  only. A static site has no backend, so there is nowhere else to put them; the
  sheet says so under the table. (#39)
- **22 Jul 2026** — Home affordability shows the house appreciating at 4%
  alongside the down payment's opportunity cost at 8%. Showing only the
  opportunity cost would be a misleading argument against ever buying. (#31)
- **22 Jul 2026** — Buy low / sell high (#44) folded into Lesson 03 rather than
  built as a standalone piece — it's an investing-behaviour topic and lands
  naturally beside the freeway/crash material.
- **22 Jul 2026** — Lesson decks kept as self-contained copies; shared
  `lesson.css` / `lesson.js` **not** extracted at three lessons. Copying is the
  primary CLAUDE.md instruction; extraction would risk a working deck for no
  visible gain. Revisit if a 4th lesson appears. (#42, #43)
- **22 Jul 2026** — At the third lesson, built `lessons.html` as the hub and
  repointed the nav "Lessons" tab + hero button at it (both index files). All
  three decks now back-link to the hub ("All lessons"); the hub backs out to the
  site. This supersedes the Batch 1 "Lesson deck back-link → Home/index.html"
  decision, now that a hub exists to be home. (#42–44, #2)
- **22 Jul 2026** — Lesson 03 two-investor demo uses one market path that always
  recovers to +40%, so crash *depth* only changes the Panicker's locked-in loss,
  never the Steady's outcome — that's the teaching point. Opportunist adds $5k of
  fresh cash at the low (called out in the disclaimer so the comparison is honest).
- **22 Jul 2026** — Tour spotlights the four nav tabs + the compound teacher only.
  The tabs stay visible at every width (the "hidden below 860px" note was stale),
  so no mobile-specific target was needed. The 5th stop targets `.comp-head`, not
  the whole `.compound` section, because a section taller than the viewport pushed
  the pop card off-screen. First visit shows a pulse on the trigger, never an
  auto-launch. (#45)
- **23 Jul 2026** — 2026 contribution limits (Roth/Trad $7,500, 401(k) $24,500) are
  **separate from the `FED` tax-bracket table**, which is still 2025. Bump each when
  its own figures are confirmed. (#49)
- **23 Jul 2026** — Compound teacher's "your age" is the anchor; `years = retire −
  age`, fully bidirectional. The master card keeps driving the *years* slider (the
  age field mirrors it), so no new master target was needed. (#49, #51)
- **23 Jul 2026** — Wealth calculator inflation is now a fixed 3% constant, slider
  removed. A visitor-set 0% inflation was the real cause of the "flaky Today's
  dollars" toggle — at a fixed 3% it always changes the number. (#50)
- **23 Jul 2026** — Wealth calculator defaults to **0% return, 0% lifestyle** so the
  opening view shows total career earnings ("your paycheck is your biggest asset");
  the visitor then adds return / lifestyle to watch wealth build. (#51)
- **23 Jul 2026** — The master card confirms on **`change` (commit), not per
  keystroke** — one native confirm per committed edit, **No** reverts. A confirm on
  every spinner tick was rejected as too noisy; typing is the primary path. (#51)
- **23 Jul 2026** — The "same money, three accounts" bar demo (Talon: confusing) was
  **replaced, not tuned** — a plain three-card `.acct3` comparison under Cash Flow,
  keyed on *when you're taxed* (now vs later). The retirement-rate select kept its
  `#rvtRate` id so the compound teacher's after-tax line still reads it cross-page. (#52)
- **23 Jul 2026** — Compliance: built **both** a first-use pop-up (once, via
  `tr-disclaimer-ok`) **and** a footer disclaimer, the safe default for the open
  question. The pop-up triggers only on **trusted** events, so the master card's
  load-time broadcast can't spring it. (#53)
- **23 Jul 2026** — Spending circuit is now **side-by-side with a reveal** (`.scircuit2`),
  not a tab toggle — typical setup always visible, the fix revealed beside it. One
  component for both home and Tools, so they can't drift (supersedes the Batch 8 tabbed
  `.scircuit`). (#54, #55)
- **23 Jul 2026** — Merged the savings-rate concept into the 1% creep test and deleted
  the standalone "Engineering" card. The test now shows the **gap visual** (income @
  raise% vs lifestyle held lower, shaded room, future-wealth badge) per
  `lifestyle-creep-gap.png`, replacing its old accumulation area chart. (#56)
- **22 Jul 2026** — Fun-page fishing egg and confetti are both **library-free**
  (rAF tween + canvas). The egg is gated to the Fun page via a `MutationObserver`
  on `#top`'s `data-active`, since its markup is fixed-position outside the page
  sections. (#46, #47)

## Not doing

- Redfin embed (#31) — technically blocked, link instead.
- Visitor-editable book list (#48) — needs a backend the site doesn't have.
- Mobile PDF in-place scrolling — iOS Safari won't scroll embedded PDFs
  reliably; "Open in new tab" is the fix (#15).
