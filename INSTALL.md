# Installation Guide

Complete setup for TACLEX DayZ on a dedicated server. Windows only.

---

## Requirements

- Windows Server (or Windows 10/11)
- DayZ Server 1.30+ installed via SteamCMD
- DayZ Tools (only if you need to recompile the PBO)
- Python 3.11+ (only if running the bridge from source)
- Optional: Groq API key (for LLM-parsed free-text commands)

---

## Folder Structure (after install)

<DayZServer>\
├── @TACLEX_SERVER\
│   ├── addons\
│   │   └── pbo_source.pbo
│   ├── configs\
│   │   ├── admins.txt
│   │   ├── vips.txt
│   │   └── dayz_config.txt
│   ├── logs\
│   │   ├── taclax.env
│   │   ├── taclax_dayz.env
│   │   ├── taclax_bridge.log
│   │   └── taclax_core.log
│   ├── RUNTIME_PACKS\
│   │   └── runtime_dayz\
│   │       ├── RCON_bridge.exe
│   │       └── recipes\
│   │           ├── KillInfected.recipe
│   │           ├── KillMilitary.recipe
│   │           ├── KillPolice.recipe
│   │           ├── TravelTo.recipe
│   │           ├── SurviveWave.recipe
│   │           ├── DefendWave.recipe
│   │           ├── HoldZone.recipe
│   │           ├── EscortVehicle.recipe
│   │           ├── EscortTruck.recipe
│   │           ├── EscortSedan.recipe
│   │           ├── HelicopterCrash.recipe
│   │           ├── Airdrop.recipe
│   │           ├── TeleportTo.recipe
│   │           └── vip\
│   │               ├── KitMedico.recipe
│   │               ├── KitMunicao.recipe
│   │               └── KitComida.recipe
│   ├── TACLEX_CORE.exe
│   ├── TACLEX_DAYZ_Control.exe
│   └── icon.ico
└── (rest of DayZ Server)

---

## Step 1 - Install the PBO

1. Copy pbo_source.pbo to:
   <DayZServer>\@TACLEX_SERVER\addons\pbo_source.pbo

2. Edit your server startup .bat file. Add:
   -serverMod=@TACLEX_SERVER

   NOT -mod. TACLEX is server-side only. Regular players do not need to subscribe or install anything.

3. Restart the DayZ server.

4. Verify in <DayZServer>\profile\DayZServer_x64_*.RPT:
   [TACLEX] HOOK OK - runtime PBO iniciando

   If you see this line, the PBO loaded correctly.

---

## Step 2 - Configure RCON

The bridge connects to the server via BattlEye RCON.

1. Open <DayZServer>\serverDZ.cfg and confirm RCON is enabled:
   BattlEye = 1;

   And in <DayZServer>\battleye\beserver_x64.cfg:
   RConPassword your_rcon_password
   RConPort 2306

2. Create or edit <DayZServer>\@TACLEX_SERVER\logs\taclax_dayz.env:
   RCON_HOST=127.0.0.1
   RCON_PORT=2306
   RCON_PASS=your_rcon_password

---

## Step 3 - Configure the CORE (optional)

The CORE interprets free-text commands via LLM (e.g. /tx travel vybor).
Without it, only keyword commands work (/tx kill, /tx airdrop, etc).

1. Get a Groq API key from https://console.groq.com/keys

2. Create <DayZServer>\@TACLEX_SERVER\logs\taclax.env:
   LLM_PROVIDER=groq
   LLM_ACTIVE_KEY=1
   LLM_MODEL=openai/gpt-oss-120b
   LLM_API_KEY_1=gsk_your_key_here

---

## Step 4 - Start the Panel

1. Run TACLEX_DAYZ_Control.exe from @TACLEX_SERVER\.

2. Wait 5 seconds. The panel detects running processes automatically.

3. Go to the Servers tab. Click Start CORE + Bridge.

4. Go to the Logs tab. You should see:
   [TAIL] monitorando logs em ...
   [OK] Bridge PID 12345
   [BRIDGE] [RCON] conectando em 127.0.0.1:2306...
   [BRIDGE] [RCON] logado
   [BRIDGE] [RCON] conectado. Ctrl+C para sair.

If RCON doesn't connect, check the password and port.

---

## Step 5 - Register your Admin GUID

Only GUIDs in admins.txt can run /tx commands.

1. Join the server with your character.

2. Open the panel Logs tab. Look for:
   [ADMIN] cache GUID: survivor -> 716f102b...

   Copy the full 32-character GUID.

3. Open the panel Admins tab. Paste the GUID (one per line):
   716f102badeb574c06a1966aff4da5ae

4. Click Save. This writes to configs\admins.txt.

5. Restart the bridge (Servers -> Stop -> Start).

6. Reconnect in-game. The bridge caches your GUID on connect.

---

## Step 6 - Register VIPs (optional)

VIPs can only run /tx medico, /tx municao, /tx comida - silent, no broadcast.

1. Open the panel VIPs tab.
2. Paste one GUID per line.
3. Click Save.

---

## Step 7 - Test

1. Join the server.

2. In chat, type:
   /tx tp 038088

   You should teleport silently to grid 038088.

3. Try a kill mission:
   /tx kill 3 military 038088

   You should see OPERACAO MILITAR / Neutralize 3 soldados hostis em 038088.

4. Kill the 3 soldiers. Reward: 2x Ammo_762x39.

---

## Editing Recipes

All recipes are plain text files in RUNTIME_PACKS\runtime_dayz\recipes\.

Two ways to edit:

Option A - Via panel (recommended):
1. Panel -> Recipes tab
2. Choose a recipe from the dropdown
3. Edit the text
4. Click Save
5. Next /tx command uses the new values (no rebuild, no restart)

Option B - Via Notepad:
1. Open the .recipe file directly
2. Edit, save
3. Next /tx command picks up changes

You can change:
- Zombie/animal class
- Spawn count and radius
- Reward item and count
- Notifications (titles, texts, intervals)
- Timeouts
- Loot table

---

## Common Configuration

Change default timeout - edit configs\dayz_config.txt:
default_duration_sec=1800

Disable Godmode - edit configs\dayz_config.txt:
godmode=false

Change broadcast interval globally - edit the [notify] section per recipe:
broadcast_interval = 60

---

## Troubleshooting

Bridge won't connect:
- Confirm RCON password in taclax_dayz.env matches beserver_x64.cfg
- Confirm port 2306 is open
- Check <DayZServer>\@TACLEX_SERVER\logs\taclex_bridge.log for errors

Commands don't trigger in-game:
- Verify your GUID is in configs\admins.txt
- Reconnect to the server after starting the bridge
- Check the panel Logs for [ROLE] 'xxx' negado

[CORE] init_only falhou: nenhuma API key:
- The CORE can't start without a valid Groq key
- Add key to taclax.env and restart
- Or ignore - keyword commands work without the CORE

PBO doesn't load:
- Check .RPT log for [TACLEX] HOOK OK
- Ensure -serverMod= is in startup (not -mod=)

Airdrop crate doesn't appear:
- Confirm recipe uses container = WoodenCrate (not SeaChest)
- Check RPT for Cannot create non-ai vehicle

Escort car disappears:
- CE allows only 1 vehicle per class
- Two escorts with same car -> older gets deleted
- Use different vehicles

---

## Stop / Restart

Stop everything:
- Panel -> Servers -> Stop CORE + Bridge
- Or run: taskkill /F /IM RCON_bridge.exe /IM TACLEX_CORE.exe

Close the panel only (keeps services running):
- Click the X on the panel window
- Bridge and CORE keep running in background

Reopen panel later:
- Run TACLEX_DAYZ_Control.exe
- It auto-detects running services and shows RUNNING

---

## Uninstall

1. Remove -serverMod=@TACLEX_SERVER from server startup
2. Delete the @TACLEX_SERVER folder
3. Restart DayZ server
4. Done

---

## Updating

1. Back up @TACLEX_SERVER\configs\ and @TACLEX_SERVER\logs\
2. Replace .exe files and recipes\ folder with new versions
3. Restart bridge via panel

---

## Support

GitHub: https://github.com/TreeoM-ManhheiM/TACLEX_Dayz
