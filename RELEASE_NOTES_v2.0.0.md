# Alek's Ultimate NX Edition v2.0.0

**The last big update.** A randomizer mode, widescreen in Dual mode, separate
music and effects volume, cheats, and a stack of quality-of-life options.

Saves and settings carry over: normal saves from 1.5.x load exactly as before.
The in-app updater installs it as usual (copy `tmc_aleks_ultimate_nx.nro` to
`/switch/tmc/` as is if installing by hand).

## New: Randomizer (Beta)

When you name a new file you now choose **NORMAL** or **RANDOMIZER**. A
randomizer file shuffles what every chest, NPC, shop, heart piece and dungeon
reward gives you. The world and the story are the same; the intro is skipped
(you start in Link's house with Ezlo). **The goal is still to defeat Vaati.**

- **Modes:** Standard, Keysanity (dungeon keys can be anywhere), Entrances
  (shuffled entrances) and Keys + Entrances. Item pool Balanced / Reduced /
  Plentiful, and **Extra checks** for out-of-the-way spots.
- **Every seed is checked before you can play it.** A second, independent
  checker walks the logic from the start and refuses any seed it cannot finish.
  24,000 test seeds (1,000 per option combination) all passed.
- **Press `−`** for the **RANDOMIZER** page: a visual tracker of what you
  found, Ezlo's notes (which region holds each key item, unlocked as you
  progress), the seed spoiler (written only if you ask for it), tunic and heart
  colours, and a diagnostic file for bug reports.
- Each randomizer save remembers its own seed. Copy, erase, autosave and
  quicksave keep the right seed with the right file. If a seed's data is
  missing or damaged the file shows an error and **never starts as a normal
  game**.
- Bombs found before the bomb bag (and arrows before the bow) give rupees
  instead of nothing.
- The seed's hash is shown when you create the file and on the file screen, so
  two players can confirm they are on the same seed.

**Why "Beta":** generation is verified, but nobody has finished a full
randomizer run on Switch yet. The logic cannot catch an in-game event that
expects the normal story order (an early build froze the Picori Festival that
way, fixed before release). If a randomizer file gets stuck, please report it
with a screenshot, the seed hash and the diagnostic file from the RANDOMIZER
page.

The randomizer core comes from [Project Picori](https://github.com/999sian/tmc)
(`bd06a639`, GPL-3.0), itself derived from the MinishMaker randomizer logic
`v1.0.0 RC 2.1`.

## New

- **Widescreen in Dual mode.** At 2X the game uses the space beside the panel
  (284 columns at the default gap). At 3X there is no spare room and it stays
  240.
- **Music and effects volume**, separately, 0-100%, in
  **SETTINGS → GAMEPLAY**.
- **Mute** Ezlo's voice, all voices, or the low-health beep.
- **Cheats** in **SETTINGS → GAMEPLAY → CHEATS**: infinite hearts and infinite
  ammo (for the current session only), heal, 20 hearts, 999 rupees, 999 shells,
  all items.
- **Autosave every** 30 s / 1 / 2 / 5 min (area changes still autosave right
  away).
- **Stick dead zone** LOW / STANDARD / HIGH in **SETTINGS → CONTROLS**.
- **`−` reopens the panel on the tab you left** (Quest, Map, Items or
  Settings) in Normal mode.

## Fixed

- **Lit torches and the lantern drew a white blob** instead of lighting the
  dark (1.5.0 report). The PPU now implements the GBA's OBJ window.
- **Link drew over the door frame on stairs** again since 1.4.14 (Link's house
  and others).
- **Widescreen:** room-to-room scrolls and area-change fades no longer drop to
  240 columns; the Minish Woods light rays reach the right edge instead of
  stopping at column 240.
- Four functions returned garbage where the GBA relied on a register value
  (a locked-door initialiser, the portal entry, two menu helpers).
- The **¿** in Spanish text no longer shows upside down.

## Includes

Every fix from 1.5.0 and 1.5.1 — see the changelog.

## Known limitations

- The randomizer is **Beta** (see above).
- Flip mode keeps the native 240 columns.
- Not addressed in this release: the Minish camera in one tunnel and the
  Dark Hyrule Castle doors from the 1.5.0 report could not be reproduced; if
  you hit them, a save from just before helps.
