# Alek's Ultimate NX Edition v1.3.8

This release closes a whole class of bugs at once instead of one enemy per
hotfix.

## Background

v1.3.71 (Eyegore), v1.3.72 (WallMaster), v1.3.73 (Mazaal) and v1.3.74
(Minish doors) were all the same defect. The original GBA code places each
enemy's and object's private state at fixed byte offsets after the shared
entity header. On the 64-bit Switch build, pointers inside that header are
twice as wide, so the game's shared code and each enemy's own code could
disagree about where a field lives — one writes a timer over another's
position, a flag is read from bytes nobody wrote, or a boss part writes past
the end of its own entity.

## What changed

Every entity type in the game (about 130 structures: enemies, bosses,
objects, NPCs, projectiles, player items) was compared byte-by-byte against
the original GBA layout and corrected. Each corrected structure now carries
a compile-time check, so this cannot silently come back.

Concrete effects you may notice:

- Enemies whose "home" position was being overwritten by their own state
  (blade traps, flying pots, leevers, keatons, lakitus, darknuts,
  madderpillars, business scrubs, gibdos, Gleerok, Moldorm, Moldworm,
  Gyorg's children and more) now wander and return the way they do on GBA.
- Enemies with state that overlapped the widened child pointer (mini slimes,
  mini fireball guys, wizzrobes, the Vaati forms, the Octorok boss) behave as
  intended — the same defect that broke Mazaal, fixed everywhere.
- Objects whose room flag was read from the wrong bytes (warp points, heart
  containers, locked/metal/boss doors, eye switches, buttons, levers,
  statues, fans, bookshelves, pots, jail bars, pressure plates, spider
  webs and others) now read the real flag, including flags set at run time.
- Moldorm and Gibdo no longer write past their own entity into the next one.
- Business scrubs see their room flag, the picolyte bottle NPC keeps its
  state, Gyorg reads its male half correctly, and per-enemy range parameters
  from room data are honoured again.

## Verification

    SHA-256  8cf76683bed662db353fe8744e52521b7a0c7cc8c4e6916b0138839cd804f372
             tmc_aleks_ultimate_nx_v1.3.8.nro
    Size     14223027 bytes

The clean release build passed native-layout, autosave-recovery, pause-map,
language, and all 26 v1.3 regression checks. Because this touches many
enemies at once, reports from real hardware — especially boss fights and
dungeon puzzles — are very welcome.
