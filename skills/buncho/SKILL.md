---
name: buncho
description: Make Japanese stories, manga (4-koma, pages, webtoon), vertical short videos, story bibles and real-city 3D scenes with BunCho from the terminal (the `buncho` CLI). Use when the user asks to write a story or script, turn a story into manga or a short video, check a manuscript for contradictions, or build a Japanese city scene. Every call is paid in DREAM from the user's BunCho account; always show the price first.
---

# BunCho CLI

BunCho turns a story into finished media: novel/script text, lettered manga,
vertical short videos (mp4), story bibles and continuity checks, and real-city
3D scenes. The `buncho` CLI calls the same API as the web app and spends DREAM
from the same wallet.

## Setup (once)

```sh
npm install -g @ai_buncho/cli      # or: npx @ai_buncho/cli <command>
# these skills: npx skills add Industrial-Dream-inc/skills
buncho login                    # opens the browser; the user signs in and approves
buncho status                   # balance, this connection's limit, auto top-up, packs
```

`buncho login` needs the user in front of a browser. Do not ask for passwords.
It opens one sign-in page. If you show the URL to the user yourself, use `buncho login --no-browser`
so only one page is open (the user picks this connection's monthly limit on that page).
If `BUNCHO_API_KEY` (bk_live_...) is set, the CLI uses it instead.

## Rules for agents (follow every time)

1. **Price first.** Before anything that spends DREAM, run `buncho cost <kind> ...`
   (same as `buncho estimate`) and tell the user the DREAM and the yen guide
   (1 DREAM ≈ ¥0.73 incl. tax). Start only after they agree.
2. **Always pass `--max-dream`** with the amount the user agreed to. The CLI
   stops before starting if the price is higher (exit code 3).
3. **Do not loop.** Each paid command runs once. The CLI adds an
   `Idempotency-Key` and retries network errors with the same key, so a retry
   is never charged twice. Do not re-run a paid command because it is slow; use
   `buncho job <id> --wait` instead.
4. **Limits are the user's.** A connection has a monthly DREAM limit (500 by
   default) chosen by the user at sign-in. A key can have a total budget,
   expiry and allowed endpoints. If a command stops with
   `connection_budget_exceeded`, `key_budget_exceeded` or exit code 3, stop and
   ask the user. Never try to change limits yourself; you cannot.
5. **Paying is the user's.** On exit code 2 (not enough DREAM), show the
   shortfall and the purchase link the CLI prints, or run
   `buncho buy --pack <id>` and give the user the link. If auto top-up is on,
   wait about a minute, run `buncho status`, and retry once.
6. Results are AI-generated. When you share them, say they were made with
   BunCho.

## Commands

| Goal | Command |
| --- | --- |
| Price (free) | `buncho cost novel --length 1000`, `buncho cost manga --story story.md --format webtoon`, `buncho cost video --story script.md` |
| Story / script text | `buncho novel create --story idea.md --mode write\|episode\|continue\|script --length 1500 --max-dream 5` |
| Manga | `buncho manga create --story story.md --format 4koma\|page\|webtoon --wait --max-dream 300` |
| Vertical short video | `buncho video create --story script.md --format shorts --wait --max-dream 900` |
| Story bible (characters, timeline, foreshadowing) | `buncho story-bible create --story manuscript.md --max-dream 10` |
| Continuity check | `buncho check --story ep02.md --bible bible.json --max-dream 10` |
| Real-city 3D (Japan) | `buncho city create --district ginza --width 400 --depth 300 --max-dream 300` |
| Job status | `buncho job <id> --wait` |
| Download | `buncho project export <job id> --format mp4\|png --out file` |
| Payment link | `buncho buy --pack pack_1000` (a person pays) |
| Receipts | `buncho purchases` |

Output is JSON on stdout; progress goes to stderr. Exit codes: 0 ok, 1 error,
2 not enough DREAM, 3 over `--max-dream`, 4 still running after `--timeout`,
64 usage error.

## Typical flows

- Story → manga: see the `buncho-novel-to-manga` skill.
- Manga/story → short video: see `buncho-manga-to-short`.
- A series with the same characters: see `buncho-series`.

## Example: "怪獣が東京を壊す5秒の動画を作って"

1. Write a 3-4 line script (scene, action, one line of dialogue or none) into
   `kaiju.md`. Keep it short: one scene of about 5 seconds.
2. `buncho cost video --story kaiju.md` → tell the user the DREAM, get OK.
3. `buncho video create --story kaiju.md --format shorts --wait --max-dream <agreed>`
4. `buncho project export <job id> --format mp4 --out kaiju.mp4`, then look at
   the first frame and the length. If a shot does not match the script, tell
   the user which one and offer to fix it in BunCho Studio (`result.editor_url`
   opens the same project; regenerating one shot is done there).
