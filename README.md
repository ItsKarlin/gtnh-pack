# Materwelon GTNH pack

The **whole** GT New Horizons client pack our server runs (now **2.9.0-RC-2**), plus our extras, kept in sync
automatically. Prism runs a small updater every time you launch the game, and it downloads whatever changed here.
When the server moves to a new GTNH version, your game follows on the next launch: new and changed mods, removed mods
and pack config/quest changes are all handled for you.

This repo holds download links and checksums for the mods (they download from GTNH's own GitHub, Maven and CurseForge
links, and no jar is re-hosted here), plus GTNH's config, quest and translation files. It contains no passwords,
tokens or world data.

## One-time setup (Prism Launcher)
1. Import the matching GTNH client pack into Prism: **Add Instance → Import** → paste
   `https://downloads.gtnewhorizons.com/Multi_mc_downloads/betas/GT_New_Horizons_2.9.0-RC-2_Java_17-26.zip`
2. Coming from an older instance? Copy `saves/`, `journeymap/`, `visualprospecting/`, `options.txt` (your keybinds)
   and `servers.dat` from its `.minecraft` into the new one. Then delete the old instance; it can't join the server
   any more.
3. Right-click the new instance → **Folder**, open `.minecraft`, and put **`packwiz-installer-bootstrap.jar`** there
   (from https://github.com/packwiz/packwiz-installer-bootstrap/releases/latest).
4. Instance → **Edit → Settings → Custom commands** → tick **Custom commands**, and set **Pre-launch command** to:
   ```
   "$INST_JAVA" -jar packwiz-installer-bootstrap.jar https://raw.githubusercontent.com/ItsKarlin/gtnh-pack/main/pack.toml
   ```
5. Launch. The first run checks ~16,000 files (a few seconds) and downloads only our extras. Then the game starts.

That's the last manual install. Future GTNH updates arrive by themselves.

**Two exceptions, announced when they happen:**
- When GTNH bumps **LWJGL3ify or Java**, Prism's instance files (outside `.minecraft`) change. The updater can't
  reach those, so that update needs a fresh import (steps 1–4 again).
- **Automagy** has no public download (it's an all-rights-reserved mod GTNH only ships in its zip). If GTNH ever
  changes its version, re-import or swap that one jar.

## Your local data is safe
The updater keeps its own list (`.minecraft/packwiz.json`) of the files it manages, and it only ever adds, updates or
removes files on that list. It never touches:
- worlds (`saves/`, including NEI bookmarks and planner plans)
- map data (`journeymap/data`)
- prospecting data (`visualprospecting/`)
- screenshots, `options.txt` and keybinds, `servers.dat`
- any mod you added yourself

**Your settings in these files are kept, even when the pack changes them** (they're only placed if you don't have
them): AE2 terminal settings, BetterQuesting settings, Waila, NEI client settings, GregTech client settings, InGameInfo,
JourneyMap settings and ServerUtilities client settings. Other config files follow the pack. That's how GTNH's own
fixes and quest updates reach you.

Tested 2026-10-12 on a fresh RC-2 import: every file was validated in place, only the 3 extras downloaded. A
simulated next version deleted a removed mod, updated a changed config, and kept a player's own AE2 setting.

## Our extras on top of GTNH
| Mod | Why |
|---|---|
| WorldEdit (GTNH) v0.0.10 | the server has it; the client half gives the wand and selection outlines |
| Foreman 0.5.4 (MIT, by Eldrinn-Elantey) | shared team task board: tasks, subtasks, assignees, HUD pins, map markers. Hotkey "Open Foreman" |
| GTNH Planner 0.2.0-alpha.5 (MIT, by Arodoid & Sbancuz) | client-only NEI flowchart factory planner. Plans save on your own PC; share one with the planner's chat-link / copy-paste buttons |

## For Karlin: updating the pack
**New GTNH version:** follow `pack-update/PACK-UPDATE.md` in gtnh-mtwn. The pack step is:
```
python3 ~/projects/GTNH-MTWN/pack-update/gtnh-pack-sync.py "<unzipped client zip>/GT New Horizons <ver>" ~/projects/gtnh-pack
# bump the version in this README + the import link, review git status, commit, push
```
It rewrites everything listed in `GENERATED.txt` and never touches our extras. It stops if a jar has no manifest
entry, and warns loudly if an unmanaged mod (`UNMANAGED.json`) changes version.

**Extras:**
```
cd ~/projects/gtnh-pack
packwiz url add "<Name>" <download-url>   # or drop a jar into mods/
packwiz refresh
git commit -am "…" && git push
```
Never put anything under `saves/`, `journeymap/data`, `visualprospecting/`, `options.txt` or `screenshots/` in the
pack. A mod that talks to the server goes on the server (`deven-pi:/home/karlin/gtnh/mods`) in the same restart, or
people can't connect.
