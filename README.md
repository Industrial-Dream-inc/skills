# BunCho skills

Skills for AI agents (Claude Code, Codex and other agents that read `SKILL.md`) that make
Japanese stories, manga, vertical short videos and real-city 3D with [BunCho](https://bun-cho.work/developers).
Every generation shows a DREAM estimate first and is paid from the user's BunCho account.

```sh
npx skills add Industrial-Dream-inc/skills
```

| Skill | What it does |
|---|---|
| `buncho` | Use the `buncho` CLI: estimate first, `--max-dream`, wait for jobs, export files |
| `buncho-novel-to-manga` | Story or novel → lettered manga (4-koma, pages, webtoon) |
| `buncho-manga-to-short` | Manga script → vertical short video |
| `buncho-series` | Keep characters and settings fixed across episodes |

## Claude Code plugin

```text
/plugin marketplace add Industrial-Dream-inc/skills
/plugin install buncho@buncho
```

The plugin adds these skills and the BunCho MCP server (`https://bun-cho.work/mcp`, OAuth).

## CLI

```sh
npx @ai_buncho/cli login --no-browser   # sign in once; you pick this connection's monthly DREAM limit
npx @ai_buncho/cli cost manga --story story.md --format webtoon
```

Pricing and limits: <https://bun-cho.work/developers/pricing.json>

日本語: AI エージェントから BunCho の小説・漫画・縦型動画・街の 3D を作るための Skills です。
作る前に必ず見積もりを出し、払うのは人です。
