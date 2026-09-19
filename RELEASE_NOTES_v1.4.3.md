# Alek's Ultimate NX Edition v1.4.3

Hotfix. Saves and settings carry over. Install by hand if you are on 1.4.1 or
older (the in-app updater works from 1.4.2 on).

## Fixed

- **Crash entering Lake Hylia — for real this time.** 1.4.2 blamed a stale
  `assets/` folder; the actual bug was in the port: the room's enemy list
  symbol was an empty placeholder instead of the real list from the ROM, so
  the game walked through memory spawning garbage until it crashed. It
  affected everyone who hasn't cleared the Temple of Droplets yet, whether
  you walk in from Lon Lon Ranch or warp with the Ocarina. Thanks
  @JJTapia19 and @digdat0 for the crash logs and saves.
- **Crash when a Hyrule Town cat swipes Minish Link.** The cat's hitbox table
  was read with the wrong pointer width (last table of its kind left in the
  port). Thanks @digdat0.

## Since 1.4.1

- In-app updater fixed (1.4.2), entity lists validated against stale asset
  trees (1.4.2), cat-girl dialogue crash, doors with the 360° stick, Ezlo box
  over the rupee counter, Pegasus dash turning with the stick (1.4.1).
