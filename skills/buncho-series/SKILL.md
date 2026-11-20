---
name: buncho-series
description: Run a multi-episode series with BunCho that keeps the same characters - fixed character sheet, continuity checks, webtoon episodes and a short promo video per episode - and redo only what failed. Use for requests like "a 10-episode romance webtoon with consistent characters, checked for contradictions, plus a 15-second promo for each episode".
---

# Series with the same characters

Plan, price, then run. Never start all episodes without the user's OK on the
total.

## 1. Plan and price (free)

1. Write or receive the series outline and the episodes (`ep01.md` ...).
2. Character sheet: `buncho story-bible create --story outline.md --max-dream <agreed>`
   → save `bible.json`. Keep names, looks (hair, eyes, clothes, age) and
   relationships fixed from here; paste the looks of the characters who appear
   at the top of every episode file.
3. Price every step and add it up for the user:
   - each episode: `buncho cost manga --story epNN.md --format webtoon`
   - each promo: `buncho cost video --story promoNN.md`
   - checks: `buncho cost check --story epNN.md`
   Tell the user the total DREAM (and ≈ yen). Mention the connection's monthly
   limit from `buncho status`; if the total is above it, ask the user to raise
   it (they reconnect and choose) or to split the run.

## 2. Run, one episode at a time

For each episode N:

1. Continuity: `buncho check --story epNN.md --bible bible.json --max-dream <per-step>`.
   If it reports contradictions, fix the text (or ask the user) before drawing.
2. Webtoon: `buncho manga create --story epNN.md --format webtoon --title "<series> #N" --wait --max-dream <per-step>`.
3. Promo (15 s): write `promoNN.md` (4-5 scenes ending on a hook), then
   `buncho video create --story promoNN.md --format shorts --wait --max-dream <per-step>`.
4. Record the job ids in `series.json` (episode, manga job, video job, status).

## 3. Redo only what failed

- A job with `status: failed` was refunded; run that one step again (a new
  command, not a loop). Leave finished episodes alone.
- A wrong panel or scene: list it for the user and fix it in BunCho Studio
  (`result.editor_url`); do not regenerate the whole episode.
- Stop and ask the user on exit code 2 or 3, or on
  `connection_budget_exceeded`.

## 4. Deliver

`buncho project export <job> --out ...` for each episode and promo, and give the
user the list (episode → webtoon pages, promo mp4).
