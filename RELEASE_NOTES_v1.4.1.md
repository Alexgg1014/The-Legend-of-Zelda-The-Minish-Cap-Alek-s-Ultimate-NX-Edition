# Alek's Ultimate NX Edition v1.4.1

Hotfix for the two GBAtemp reports on 1.4.0, plus the Ezlo box fix and one
new quality-of-life option. Save files and settings from 1.4.0 carry over.

## Fixed

- **Crash talking to the girl with the cat (Hyrule Town) after the library
  quest** — present since 1.3. Her dialogue calls a function by GBA ROM
  address; the port resolved every other townsperson dialogue but not this
  one, and jumped to the raw address. `CALL_FUNC` dialogues now always go
  through the port's function table (townsperson NPCs and the script
  interpreter), and an unresolved one is logged instead of executed.
- **Link getting stuck walking into doors with 360° stick movement.** Door
  and room-transition triggers are 6-pixel rectangles meant to be walked
  into straight; a stick 11° off an axis made Link slide along the frame.
  The two directions on either side of each axis now snap to it (a 22.5°
  cone); the other 24 directions are untouched, so diagonal walking feels
  the same.
- **Widescreen: the TALK TO EZLO box overlapping the rupee / FPS counters.**
  HUD columns to the right of the text box stay anchored to the screen edge.

## Added

- **PEGASUS TURN WHILE DASHING** (SETTINGS → GAMEPLAY → QUALITY OF LIFE, on
  by default). The original only leans a dash 11° and stops it when you
  press the opposite direction. With this on, the dash turns toward the
  stick at 11° per frame — a corner is an 8-frame arc, a U-turn 16 —
  Link's facing and the dash-sword hitbox follow, and only releasing the
  item or hitting a wall ends the run. Switch it off for the original
  behaviour.

## Notes

- Updating the NRO does not refresh the `assets/` folder on the SD card; this
  release needs no new assets.
- Randomizer and Dual-mode widescreen are still planned for later releases.
