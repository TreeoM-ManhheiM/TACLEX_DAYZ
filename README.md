# TACLEX DayZ

**Server-side dynamic mission system for DayZ 1.30 + Badlands**

Admin types `/tx` in chat → mission triggers in-game. No client mod required.

---

## Overview

TACLEX DayZ turns a vanilla DayZ server into a dynamic mission platform. An admin sends a simple command via chat, and the system spawns zombies, animals, vehicles, crates, or runs timed events — all server-side, no client downloads.

Built on top of the TACLEX ML engine, the same core powers Arma 3 and DayZ Reforger with minimal adapter changes.

---

## Event Types

| Command | Effect | Reward |
|---------|--------|--------|
| `/tx kill N infected\|military\|policia <grid>` | Kill N zombies | SKS / Ammo_762 / Ammo_9x19 |
| `/tx travel <grid>` | Player reaches location | SKS (at grid) |
| `/tx survive <sec> <grid>` | Survive wolf pack | SalineBagIV ×2 |
| `/tx defend <grid>` | Zombie waves | Ammo_556x45 ×3 |
| `/tx hold <grid>` | Hold zone for time | Tetracycline ×1 |
| `/tx pickup <item> <grid>` | Find item | The item itself |
| `/tx escort <gridA> <gridB>` | Drive vehicle from A to B | SKS / M4A1 / AK74 |
| `/tx airdrop <grid>` | Crate with loot | M4A1 + 2× mags |
| `/tx tp <grid>` | Silent teleport (admin only) | — |

**VIP commands** (silent, no broadcast):
`/tx medico`, `/tx municao`, `/tx comida`

All missions run in parallel (pool of 10). Grid format: 6 digits (`038088`) or 8 digits (`03800880`).

---

## Features

- **Universal grid** – 6 or 8 digits, precise to 100m or 10m
- **Parallel missions** – up to 10 running simultaneously
- **Three-tier roles** – admin, VIP, public
- **Live recipe editing** – change spawns, rewards, texts without rebuild or restart
- **Per-mission notifications** – fully customizable titles, broadcasts, timeouts
- **No client mod** – pure server-side, players connect with vanilla DayZ
- **Open source** – MIT license, contributions welcome

---

## Architecture
