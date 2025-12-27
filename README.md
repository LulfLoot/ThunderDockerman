# ThunderModMan

A lightweight, web-based mod manager for game servers. Browse and install mods from [Thunderstore](https://thunderstore.io) directly through your browser.

![Docker](https://img.shields.io/badge/docker-ready-blue?logo=docker)
![License](https://img.shields.io/badge/license-MIT-green)

## Features

- 🎮 **Valheim Dedicated Support** – Optimized for Valheim Docker servers (also supports Lethal Company, RoR2, etc.)
- 🔍 **Browse & search** – Find mods directly from Thunderstore
- ⚡ **One-click install** – Automatic download and extraction to your server
- 📦 **Dependency handling** – Installs required dependencies automatically
- 🔄 **Server control** – Start, stop, and restart your game server from the UI
- 🎨 **Modern UI** – Dark theme with tabbed navigation and inline console

## Quick Start

### Option 1: Standalone (Recommended)

Use with your existing game server:

```bash
docker run -d \
  --name thundermodman \
  -p 9876:9876 \
  -v /path/to/your/bepinex/plugins:/mods \
  -v thundermodman-data:/data \
  thundermodman:latest
```

Then open **http://localhost:9876** to manage mods.

### Option 2: With Example Valheim Server

Clone the repo and set up your personal configuration:

```bash
git clone https://github.com/<username>/thundermodman.git
cd thundermodman

# Copy example configs to your personal configs
cp example.env .env
cp example.docker-compose.yml docker-compose.yml

# Edit .env with your settings (server name, password, etc.)
nano .env

# Start the stack
docker compose up -d
```

> **Note:** Your personal `.env` and `docker-compose.yml` files are gitignored, so you can safely `git pull` updates without overwriting your settings.

## Configuration

### Environment Variables

| Variable            | Default           | Description                                |
| ------------------- | ----------------- | ------------------------------------------ |
| `PORT`              | 9876              | Web UI port                                |
| `MODS_DIR`          | /mods             | Directory to install mods                  |
| `DATA_DIR`          | /data             | Data directory for tracking installed mods |
| `RESTART_CONTAINER` | -                 | Container name for server control          |
| `SERVER_NAME`       | My Valheim Server | Game server display name                   |
| `WORLD_NAME`        | MyWorld           | World/save name                            |
| `SERVER_PASSWORD`   | changeme          | Server password                            |
| `TZ`                | UTC               | Timezone                                   |

### Enabling Server Control

To allow ThunderModMan to start, stop, and restart your game server:

```bash
docker run -d \
  --name thundermodman \
  -p 9876:9876 \
  -v /path/to/bepinex/plugins:/mods \
  -v thundermodman-data:/data \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -e RESTART_CONTAINER=your-game-server-container \
  thundermodman:latest
```

## Supported Games

Any game supported by Thunderstore with BepInEx mods:

- **Valheim** (Primary Priority)
- Lethal Company
- Risk of Rain 2
- Content Warning
- GTFO
- Vintage Story
- _...and many more_

## Volume Mount Examples

| Game           | Container         | Mod Path                              |
| -------------- | ----------------- | ------------------------------------- |
| Valheim        | mbround18/valheim | `/home/steam/valheim/BepInEx/plugins` |
| Valheim        | lloesche/valheim  | `/config/bepinex/plugins`             |
| Lethal Company | -                 | `/game/BepInEx/plugins`               |

## Development

Run locally without Docker:

```bash
npm install
npm run dev
```

The server will start on http://localhost:9876

## API

| Method | Endpoint                             | Description          |
| ------ | ------------------------------------ | -------------------- |
| GET    | `/api/communities`                   | List supported games |
| GET    | `/api/packages/:community`           | Get mods for a game  |
| GET    | `/api/packages/:community/search?q=` | Search mods          |
| GET    | `/api/installed`                     | List installed mods  |
| POST   | `/api/install`                       | Install a mod        |
| DELETE | `/api/uninstall/:fullName`           | Uninstall a mod      |
| POST   | `/api/start-server`                  | Start game server    |
| POST   | `/api/stop-server`                   | Stop game server     |
| POST   | `/api/restart-server`                | Restart game server  |
| GET    | `/api/server-status`                 | Get server status    |
| GET    | `/api/server-logs`                   | Get server logs      |

## License

MIT
