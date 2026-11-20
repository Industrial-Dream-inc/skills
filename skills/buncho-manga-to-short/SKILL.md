---
name: buncho-manga-to-short
description: Make a vertical short video (9:16 mp4, for Shorts/Reels/TikTok promos) from a story, script or the same text used for a manga, with BunCho. Use when the user wants a short drama or a teaser for a manga episode. Requires the buncho CLI.
---

# Story or manga → short video

1. Write the video script: 4-8 short scenes, each with what is seen and at most
   one line of dialogue. For a teaser of a manga episode, take the episode's
   hook and end on a question. 15 seconds ≈ 4-5 scenes.
   (`buncho novel create --story episode.md --mode script --length 400 --max-dream <agreed>`
   can draft it.)
2. Price: `buncho cost video --story script.md` → tell the user, get OK.
3. `buncho video create --story script.md --format shorts --title "<title>" --wait --max-dream <agreed>`
4. `buncho project export <job id> --format mp4 --out teaser.mp4`. Watch it or
   at least check the length and the first frame. Report any scene that does
   not match the script; regenerating one scene is done in BunCho Studio
   (`result.editor_url`).

The video is made from the script text. Making a video directly from an
existing manga project id is not available in the CLI yet; reuse the same
story text and the characters from the story bible.
