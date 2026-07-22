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

## Batch 2 — Master income & age control ⚠️ DO BEFORE ANY CALCULATOR WORK

- [ ] 16. One income + age input placed directly **above** the "Your savings
      potential" calculator, that auto-populates every calculator on the site.
      - Editing a value **inside** an individual calculator must NOT feed back
        to the master or to other calculators — master is the only broadcaster.
      - Persist via `localStorage` so the lesson decks (separate files) pick it up.
      - Note: "wealth building calculator" in the docx always means
        **"Your savings potential"**.

---

## Batch 3 — Wealth-building calculator overhaul

Biggest single job. Keep the current default view exactly as-is; new features
live behind an "Advanced" toggle.

- [ ] 17. Add "Advanced" tab that reveals the new features below
- [ ] 18. Taxes — use **real bracket math**, not a flat rate (confirmed)
- [ ] 19. Inflation slider — **the current one does nothing**; make it work
- [ ] 20. Compound interest on invested savings only
- [ ] 21. Hover a bar → show that year's number (add to the **default** view, not
      just Advanced — Talon considers this important)
- [ ] 22. Lifetime spending total at the bottom — **must NOT compound**
      (it isn't invested)
- [ ] 23. Year 1 / Year 10 / Year 20 income steps, as a collapsed drop-down under
      Year 1 income so it takes no space unless opened. Replaces the assumption
      of a flat 4% raise every year for a whole career.

Screenshot `image11.png` shows only *how another site handled tax* — do **not**
copy its debt or lifestyle handling.

---

## Batch 4 — Simple new calculators

Same build pattern, one thread.

- [ ] 24. **Lump sum calculator** → Tools ▸ Cash Flow. Investment slider (cap $5M,
      default $250,000) + years slider (up to 47, so an 18-year-old reaches 65).
- [ ] 25. **Tax bracket calculator** → Tools ▸ Tax & Debt
- [ ] 26. **Passive income calculator** — $1M at 10% funds $100k/yr forever.
      Show that the money never runs out.
- [ ] 27. **Death benefit income replacement** → Tools ▸ Protection. Same logic as
      #26 but with a $1M death benefit replacing a salary. Must mirror the
      version in the lesson deck.

---

## Batch 5 — Complex new calculators

- [ ] 28. **Lifetime Roth toggle.** Keep the existing one-time-contribution view as
      the default; add a toggle for "contribute this every year until retirement".
      Hold contributions **flat** for now, with a note that limits may rise.
- [ ] 29. Add **Traditional IRA and 401(k)** options with their respective
      contribution limits.
- [ ] 30. **Roth vs Traditional vs 401(k) demonstration** — small visual showing
      the difference between the three.
- [ ] 31. **Home affordability calculator** → Tools ▸ Liquidity & Assets.
      Interest rate, average down payment, fees, and the opportunity cost of
      that down payment had it stayed invested. Include an "optimal scale" with
      *retire early* as one option. Plus a plain link to Redfin.
      **Redfin embed is not possible** — no public embed, and their terms forbid
      iframing. Link only. (Confirmed with Talon.)
- [ ] 32. **Risk tolerance survey** — short and clickable, scoring into
      Conservative / Moderate / Growth / Aggressive Growth.
      **Must be written from scratch** — published surveys are copyrighted.

---

## Batch 6 — Protection section & scorecards

- [ ] 33. Rebuild "What's your level of protection" to match `image13/14.png`.
      Ignore the warning pop-up in the screenshot. Make **Protection Overview
      Scorecard** the top drop-down.
- [ ] 34. Term vs whole life graphs render distorted on web and mobile — fix or
      rebuild from scratch.
- [ ] 35. Move every scorecard to the **top** of its section.
- [ ] 36. Move "Savings & wealth building scorecard" into **Cash Flow**.
- [ ] 37. New scorecards for **Retirement, Tax, and Liquidity**.
- [ ] 38. Port the life insurance qualification + disability gap calculators from
      `lesson-protection.html` into Tools ▸ Protection. Add a field for
      **current disability insurance** (see `image3.png`).

---

## Batch 7 — Balance sheet pop-ups

- [ ] 39. Make the balance sheet (`#balancesheet`, ~line 1022) interactive —
      click a line to open a pop-up and enter numbers.
      Layout ideas in `image4–8.png`; treat as inspiration, not spec.

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

- Batch 3 #23 — confirm the year 10 / year 20 incomes should interpolate between
  steps rather than jump.

## Decisions log

- **22 Jul 2026** — Tutorial: spotlight-the-tabs, not a guided scroll tour.
  Explore section deleted, trigger moves to the hero. (#45)
- **22 Jul 2026** — Wealth calculator taxes use real bracket math, not a flat rate.
- **22 Jul 2026** — Roth contributions held flat, with a note that limits may rise.
  Traditional and 401(k) added alongside, with their own limits.
- **22 Jul 2026** — Redfin embed dropped; plain link instead.
- **22 Jul 2026** — Book suggestions handled by text/email, not a form.
- **22 Jul 2026** — Risk tolerance survey written from scratch (copyright).

## Not doing

- Redfin embed (#31) — technically blocked, link instead.
- Visitor-editable book list (#48) — needs a backend the site doesn't have.
- Mobile PDF in-place scrolling — iOS Safari won't scroll embedded PDFs
  reliably; "Open in new tab" is the fix (#15).
