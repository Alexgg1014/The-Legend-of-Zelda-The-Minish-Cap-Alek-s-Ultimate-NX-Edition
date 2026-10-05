# Alek's Ultimate NX Edition v1.5.1

Four fixes, all reported by testers on 1.5.0. Saves and settings carry over; the
in-app updater installs it as usual (copy `tmc_aleks_ultimate_nx.nro` to
`/switch/tmc/` as is if installing by hand).

## Fixed

- **Crash near a townsperson in Percy's house** (Data Abort at address `0x6`).
  A script takes that NPC's hitbox away (`townsperson.c`), but the NPC stays in the
  list of things Link can interact with, and the interaction check read the
  hitbox width without looking. On the GBA that address is the BIOS and the read
  is harmless; on the Switch it is a crash. A character without a hitbox is now
  skipped. This is the most likely cause of the crash reported when lighting the
  lantern in that house.
- **Crash when duplicating Link in the elemental room.** A Four Sword clone is
  still in its init action, with no hitbox, while it stands on a button that
  pushes every clone. The tile-collision routine now returns when the entity has
  no hitbox, and an uninitialised variable in the clone-tile code is zeroed.
  Thanks @digdat0 for the report and the crash log. *(Fixed from the log; not
  reproduced on hardware.)*
- **The small lily pads of Castor Wilds did not move.** The three rail scripts
  they follow live in a ROM table that came out empty on this build, so every
  pad had a null rail. The tables are now built in. Reported by @digdat0.
- **Royal Valley did not light up without the lantern.** The list of lit regions
  is read until a terminator, and the terminator was a separate constant that
  only happened to sit next to the list on the GBA. On the Switch the read ran
  past the list and picked up unrelated data as extra regions. The terminator is
  now inside the list. Reported by @digdat0.

## Known limitations

- Widescreen works in NORMAL mode only; DUAL and FLIP keep the native 240
  columns.
- Separate music and effects volume is not in this release.
