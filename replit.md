# Minecraft AFK Bot

## Overview
A Minecraft AFK bot that keeps an Aternos server online 24/7 using [Mineflayer](https://github.com/PrismarineJS/mineflayer). It connects to a Minecraft server, performs anti-AFK movements, auto-reconnects on disconnect, and serves a web dashboard.

## Architecture
- **Runtime**: Node.js 20
- **Main entry**: `index.js` (2080 lines — bot logic + Express web server)
- **Config**: `settings.json` — server IP, bot account, modules, movement, combat, etc.
- **Logger**: `logger.js` — in-memory log buffer (last 300 entries), also prints to stdout
- **Accounts**: `launcher_accounts.json` — Microsoft/offline account cache

## Key Dependencies
- `mineflayer` — Minecraft bot framework
- `mineflayer-pathfinder` — pathfinding for movement
- `minecraft-data` — Minecraft game data
- `express` — HTTP dashboard server

## Web Dashboard
- Runs on port **5000** (0.0.0.0)
- Route `/` — HTML dashboard showing bot status, uptime, logs
- Route `/ping` — health check (returns "pong")
- Route `/api/status` — JSON bot status
- Route `/api/logs` — JSON recent logs
- Route `/api/settings` — GET/POST bot settings

## Bot Features
- Auto-connect and auto-reconnect with exponential backoff
- Anti-AFK: circle walking, random jumps, look-around
- Auto-auth (`/login` command support)
- Chat messages on a configurable repeat schedule
- Combat module (attacks nearby mobs)
- Discord webhook notifications (optional)
- Self-ping keepalive (for Render.com hosting — disabled locally)

## Configuration (`settings.json`)
- `server.ip` / `server.port` — target Minecraft server
- `bot-account.username` / `type` — "offline" or "microsoft"
- `utils.auto-reconnect` — enables reconnect on disconnect
- `utils.chat-messages` — periodic chat message list
- `movement` — circle-walk, look-around, random-jump settings
- `discord` — optional webhook URL and event toggles

## Workflow
- **Start application**: `npm start` → `node index.js`, port 5000 (webview)

## Deployment
- Target: **vm** (always-running — required for a persistent bot)
- Run: `node index.js`
