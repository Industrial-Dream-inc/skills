---
name: buncho-novel-to-manga
description: Turn a novel, chapter or short story into lettered manga with BunCho (4-koma, pages or vertical webtoon). Use when the user has prose and wants it as manga or webtoon. Requires the buncho CLI (see the buncho skill).
---

# Novel → manga

1. Put the text in a file (one chapter per run; long novels: one episode at a
   time). Japanese or other languages are fine.
2. Optional but recommended for anything longer than one episode:
   `buncho story-bible create --story chapter.md --max-dream <agreed>` and keep
   the JSON. Use the character descriptions in it so every episode looks the
   same.
3. Price: `buncho cost manga --story chapter.md --format webtoon` (or `page`,
   `4koma`). Tell the user the DREAM and get OK.
4. `buncho manga create --story chapter.md --format webtoon --title "<title>" --wait --max-dream <agreed>`
5. Check the result: `result.pages` are the lettered pages (images). Open them
   and check that panels follow the story and characters look consistent. If a
   panel is wrong, tell the user which page/panel; fixing one panel is done in
   BunCho Studio at `result.editor_url` (or with the MCP tool `edit_panel`).
6. Save: `buncho project export <job id> --format png --out chapter.png`.

Formats: `4koma` (four-panel strip), `page` (Japanese manga page), `webtoon`
(vertical scroll). Do not start the next episode until the user has seen the
previous one, unless they asked for the whole run up front and agreed to the
total price.
