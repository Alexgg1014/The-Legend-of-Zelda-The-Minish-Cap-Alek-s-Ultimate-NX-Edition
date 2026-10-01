# Alek's Ultimate NX Edition v1.5.0

**The first release confirmed completable from the beginning through the final
credits on Nintendo Switch hardware.** The final sequence and the credits were
played through end to end. That is not a promise of zero bugs, but the ending
no longer stops you.

Saves and settings carry over; the in-app updater installs it as usual (copy
`tmc_aleks_ultimate_nx.nro` to `/switch/tmc/` as is if installing by hand).

## Fixed

- **The credits crashed the moment they started.** Two tables the staff roll
  walks held 32-bit GBA code pointers that the Switch read as 64-bit ones, so
  two neighbouring addresses fused into a jump to nowhere. They now point at the
  real handlers. Thanks @mnavarretem86 for the report and for playing it through.
- **Shortcuts only fire what the save you are playing owns.** X / Y / ZL / ZR
  assignments are shared by every save file, so a slot could name an item the
  current file never found, and use it. An assignment now stays inert until the
  save has the item, and an upgrade (bombs to remote bombs, bow to light
  arrows, boomerang, shield, lit lantern) keeps its slot.
- **Widescreen:** a seam on the top row of a room while the screen shakes.
- A corrupted item entity is now dropped before it can read outside its table.

## New

- **The shortcuts are on the game screen.** Each of X / Y / ZL / ZR shows the
  equipped item on a button badge along the bottom-left, with bomb and arrow
  counts, in NORMAL, DUAL and FLIP. A shortcut with nothing in it shows
  nothing, so the HUD looks vanilla until you use them. Choose FULL, ICONS or
  OFF under **SETTINGS → DISPLAY → SHORTCUT HUD**. Button art by
  **lbsbezerra**.
- **Thanks at the end of the credits** for mnavarretem86 and lbsbezerra, and a
  closing page with the Alek's Ultimate title.

## Includes

Every fix from the 1.4.x line up to 1.4.18 (the Vaati Reborn skip and the Vaati
Transfigured loop, the four-cannon puzzle, the dojo braziers, the Gyorg and
Octorok boss rooms and more) — see the changelog.

## Known limitations

- Widescreen works in NORMAL mode only; DUAL and FLIP keep the native 240
  columns.
- Separate music and effects volume is not in this release.
