# Materwelon GTNH extras

The mods our server runs **on top of** GT New Horizons 2.9.0-RC-1, kept in sync automatically.
Prism runs a small updater every time you launch the game, and it downloads whatever changed here.
It only touches files it put there itself; the normal GTNH mods are never changed.

This repo is only a list of download links and checksums. It contains no passwords, tokens or world data.

## One-time setup (Prism Launcher)
1. Right-click your GTNH 2.9.0-RC-1 instance → **Folder**, open `.minecraft`.
2. Put **`packwiz-installer-bootstrap.jar`** in that `.minecraft` folder
   (from https://github.com/packwiz/packwiz-installer-bootstrap/releases/latest).
3. If you already have a mod from the list below, leave it. The updater checks it and keeps it if it's the same file.
4. Instance → **Edit → Settings → Custom commands** → tick **Custom commands**, and set **Pre-launch command** to:
   ```
   "$INST_JAVA" -jar packwiz-installer-bootstrap.jar https://raw.githubusercontent.com/ItsKarlin/gtnh-pack/main/pack.toml
   ```
5. Launch. A small window shows the update, then the game starts.

## Your local data is safe
The updater keeps its own list (`.minecraft/packwiz.json`) of **only the files it installed**, and it only ever adds,
updates or removes files on that list. Everything else is never touched: worlds (`saves/`), maps (`journeymap/`),
prospecting data (`visualprospecting/`), screenshots, `options.txt` and keybinds, your configs, and all other mods.
Tested on 2026-10-08 with fake copies of each of those: every file's checksum was identical after an update.

## What's in it
| Mod | Why |
|---|---|
| WorldEdit (GTNH) v0.0.10 | the server has it; the client half gives the wand and selection outlines |
| Foreman 0.5.4 (MIT, by Eldrinn-Elantey) | shared team task board: tasks, subtasks, assignees, HUD pins, map markers. Hotkey "Open Foreman" |

## For Karlin: changing the pack
```
cd ~/projects/gtnh-pack
packwiz url add "<Name>" <download-url>   # or drop a jar into mods/
packwiz refresh
git commit -am "…" && git push
```
**Config files must never overwrite a player's own settings.** If you ship one, mark it preserved, so it is only
placed when the player doesn't have that file yet:
```
# in index.toml, on that file's [[files]] entry (packwiz refresh keeps it)
preserve = true
```
Only ship jars and preserved configs. Never put anything under `saves/`, `journeymap/`, `visualprospecting/`,
`options.txt` or `screenshots/` in the pack.

Put the same jar on the server (`deven-pi:/home/karlin/gtnh/mods`) at the same time. A mod that talks to the server
must go on both sides in the same restart, or people can't connect.
