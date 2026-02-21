# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

This is the **planning and documentation repository** for Gravity Claw — a lean, secure personal AI agent built from scratch in TypeScript. Source code scaffolding begins at Level 1 (see Build Levels below).

### `doc/` Directory

| File | Contents |
|---|---|
| `gravity-claw.html` | Full architecture reference document |
| `gravity-claw-initialisation-prompt.md` | Prompt to paste into an AI coding agent to bootstrap the project from scratch |
| `update-gravity-claw-railway-sop.md` | Step-by-step Railway deploy/manage SOP |

## Project Architecture

Gravity Claw is a **Telegram bot** with an agentic LLM loop, built with these non-negotiable constraints:

- **No web server / no open ports** — Telegram long-polling only
- **User ID whitelist** — silently ignore all Telegram users except the owner's ID
- **Secrets in `.env` only** — never in code, logs, or memory files
- **MCP-only integrations** — no community skill files; use Model Context Protocol servers (separate auditable processes)
- **Max iteration limit** on the agent loop; dangerous shell commands require explicit confirmation

### Tech Stack

| Package | Role |
|---|---|
| `grammy` | Telegram bot framework |
| `@anthropic-ai/sdk` | Primary LLM (Claude) |
| `better-sqlite3` + FTS5 | Short-term/local memory |
| `openai` SDK | Whisper transcription (STT) |
| ElevenLabs API | Text-to-speech |
| `tsx` | Dev runner with hot-reload (`tsx watch`) |
| `node-cron` | Heartbeat/proactive check-ins |

### Memory Tiers

1. **Core Memory** — injected into system prompt (`soul.md`)
2. **Conversation Buffer** — SQLite (resets on Railway deploy)
3. **Semantic Long-Term** — Pinecone vector DB (persists across deploys)

### Key Files

| File | Purpose |
|---|---|
| `src/index.ts` | Entry point — initializes bot and agent loop |
| `soul.md` | Bot personality / core memory injected at startup |
| `mcp.json` | MCP server definitions |
| `tsconfig.json` | TypeScript config |
| `.env` | All secrets (not committed) |

## Development Workflow

### Local Development

Before testing locally, always pause Railway first to avoid two bot instances fighting over the same Telegram token:

```bash
railway down          # pause Railway service
npm run dev           # tsx watch src/index.ts (hot-reload)
```

### Type-check Before Deploying

```bash
npx tsc --noEmit
```

### Deploy to Railway

```bash
railway up --detach   # triggers Docker build (~60–90s)
railway logs --lines 40
```

Healthy startup logs include:
- `✅ Soul loaded (soul.md)`
- `✅ Connected as @your_bot_name`
- `✅ Heartbeat scheduled`

### Railway CLI Quick Reference

```bash
railway down                          # pause live bot
railway up --detach                   # deploy
railway logs --lines 100              # view logs
railway variables set KEY="value"     # set env var
railway variables                     # list env vars
railway open                          # open dashboard
```

## Build Levels

When implementing features, respect the intended build order:

1. **Level 1 — Foundation**: Telegram + Claude + basic agent loop (start with one tool, e.g. `get_current_time`)
2. **Level 2 — Memory**: SQLite + FTS5 + memory read/write tools
3. **Level 3 — Voice**: Whisper transcription in, ElevenLabs TTS out
4. **Level 4 — Tools**: MCP bridge (shell, files, Gmail, Notion, GitHub, Supabase)
5. **Level 5 — Heartbeat**: `node-cron` for daily briefings and proactive check-ins

## Deployment Notes

- **Dockerignore**: `.env`, `node_modules/`, `gravity-claw.db` are excluded from Railway builds
- **Docker includes**: `src/`, `tsconfig.json`, `soul.md`, `mcp.json`, `package.json`, `package-lock.json`
- SQLite (`gravity-claw.db`) resets on every Railway deploy — it is ephemeral. Only Pinecone memory persists.

## Troubleshooting

| Problem | What to do |
|---------|------------|
| Build failed | `railway logs --lines 100` — look for npm or TypeScript errors |
| Bot crashes on startup | Check for missing env vars: `railway variables` |
| Messages going to wrong instance | Two instances running — `railway down` before `npm run dev` |
| Need to rollback | Fix locally, then `railway up --detach` again |
