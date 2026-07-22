# Website edit list

Source: `Prompts for Claude Pt.3.docx` (22 Jul 2026). 45 items, grouped into batches.

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

## Batch 8 — Cash flow section rework

- [ ] 40. "Interrupt the spending circuit": show the original
      *income → checking → lifestyle* flow first, then progress to
      *income → wealth building → checking → lifestyle*.
- [ ] 41. Lifestyle-creep visual renders badly — rebuild it. Whatever design lands
      in the Batch 9 cash-flow lesson should be reused here (and in
      "engineering an increased savings rate").

---

## Batch 9 — Lessons 02 & 03

Separate files — these never touch `index.html`, so this is the cheapest batch.

- [ ] 42. **Lesson 02 — Cash flow.** Core topic: the power of a wealth-building
      account (reuse the site's existing demo). Also: dangers of lifestyle creep
      (`image12.png`), 401(k) matches and work benefits, a simple explanation of
      what a rollover is, and the passive income calculator from #26.
- [ ] 43. **Lesson 03 — Investing & risk.** Built around the freeway analogy:
      - Right lanes near entrances/exits = short-term money (1–3 yrs), slow and
        congested because people are entering and exiting.
      - Middle lanes = medium term (~5 yrs), faster, but be ready to exit on time.
      - Far-left lanes = retirement accounts, full speed, highest risk tolerance.
      - HOV lane = having a financial planner alongside you.
      - A crash slows everyone regardless of lane — uncontrollable, and inevitable.
      Talon's line: *"Great stock risk and return analogy. I'm the Google Maps."*
      Numbers in the docx are illustrative — verify before using.
- [ ] 44. **Buy low / sell high** — find a good analogy with strong visuals.

---

## Batch 10 — Fun page & tutorial

- [ ] 45. **Site tutorial — spotlight the tabs.** DECIDED 22 Jul 2026.
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
- [ ] 46. Easter egg: a $10 bill on a fishing hook that swims across the screen.
      Clicking it offers to help open a Roth IRA, with Talon contributing $10.
      **Fun page only.**
- [ ] 47. Red "DO NOT PRESS" button → full-screen confetti celebration.
- [ ] 48. Favourite books: let visitors suggest additions.
      **A static site can't accept submissions** — resolved as text/email
      (confirmed with Talon).

---

## Open questions

- None outstanding.

## Decisions log

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

## Not doing

- Redfin embed (#31) — technically blocked, link instead.
- Visitor-editable book list (#48) — needs a backend the site doesn't have.
- Mobile PDF in-place scrolling — iOS Safari won't scroll embedded PDFs
  reliably; "Open in new tab" is the fix (#15).
