# TACLEX DayZ

Server-side dynamic mission system for DayZ 1.30 + Badlands.

Admin types `/tx` in chat → mission triggers in-game. No client mod needed.

## What It Does

9 event types controlled via RCON:

| Command | Effect |
|---|---|
| `/tx kill N infected\|military\|policia <grid>` | Kill N zombies |
| `/tx travel <grid>` | Player walks to location |
| `/tx survive <sec> <grid>` | Survive wolf pack |
| `/tx defend <grid>` | Zombie waves |
| `/tx hold <grid>` | Hold zone for time |
| `/tx pickup <item> <grid>` | Find item |
| `/tx escort <gridA> <gridB>` | Drive vehicle |
| `/tx airdrop <grid>` | Crate with loot |
| `/tx tp <grid>` | Teleport (admin only) |

Plus VIP commands (silent): `/tx medico`, `/tx municao`, `/tx comida`

## Features

- **Universal grid** — 6 digits (`038088`) or 8 digits (`03800880`)
- **Parallel missions** — up to 10 running at once
- **3-tier roles** — admin, VIP, public
- **Editable recipes** — change spawns, rewards, texts in real-time (no rebuild)
- **Per-mission notifications** — configurable titles, broadcasts, timeouts
- **Role enforcement** — public cannot trigger admin recipes

## Requirements

- DayZ Server 1.30+
- Windows Server
- Python 3.11+ (only if running bridge from source)
- Optional: Groq API key (for LLM-parsed commands)

## Architecture
