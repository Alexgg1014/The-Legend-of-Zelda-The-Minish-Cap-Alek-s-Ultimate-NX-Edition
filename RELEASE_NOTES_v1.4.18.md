# Alek's Ultimate NX Edition v1.4.18

Saves and settings carry over; the in-app updater installs it as usual (copy
`tmc_aleks_ultimate_nx.nro` to `/switch/tmc/` as is if installing by hand).

## Fixed

- **The final battle skipped Vaati Reborn, and Vaati Transfigured started over
  every time you beat it.** One cause for both. The port carries a guard that
  resumes the defeat sequence when Vaati Reborn's phase counter is already past
  the last phase, for a phase-3 boss restored from an old quick/autosave. It ran
  from the entity's very first frame, and the boss spawns with the room and sits
  idle until the intro cutscene initialises it, so until that moment the counter
  holds whatever the recycled entity slot had. Measured on the reporter's save:
  the guard fired on frame one, forced the defeat sequence, and that sets the
  room flag the "Vaati defeated" cutscene waits for, which warps you straight
  into Vaati Transfigured. The intro never finishes, so the flag that marks it
  done is never set, and on the way back from Transfigured the same misfire
  happens again before the boss can delete itself - which is the loop. The guard
  now only runs once the boss is actually in the fight. Thanks @mnavarretem86
  for the report, the video and the save.

  If your file is already stuck in that loop it recovers on its own with this
  build. If the Vaati-and-Zelda scene never registered on your file, walk back
  down to the Zelda statue platform: that scene replays when its flag is clear,
  and the sequence picks up from there.

## Since 1.4.17

- Assigning an item to X / Y / ZL / ZR from the pause screen no longer also
  changes what is on A and B.
