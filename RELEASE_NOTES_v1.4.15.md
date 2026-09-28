# Alek's Ultimate NX Edition v1.4.15

Saves and settings carry over; the in-app updater installs it as usual (copy
`tmc_aleks_ultimate_nx.nro` to `/switch/tmc/` as is if installing by hand).

## Fixed

- **Dark Hyrule Castle: the four-cannon puzzle could not be completed.** All
  four Links reflect their cannonball, but only the two statues on the left
  were ever destroyed. The reflected ball looks up the statues through a
  shortcut that lands in the right place only on the GBA; here it started 16
  bytes early, so of the four slots it read, two were unrelated manager data
  and only two were real statues - always the same two. It now reads the real
  list. Thanks @mnavarretem86 for the report, the video and the save.

  Measured in this build: the shortcut points at +0x28 while the statue list
  lives at +0x38, which is exactly the two-of-four behaviour in the video. The
  puzzle itself was not driven end to end here (it needs the four clones and a
  charged spin), so if anything still misbehaves in that room, please say so.

## Since 1.4.14

- Dojo light-manager crash, the Gyorg Pair final phase, and the hardening pass
  over Vaati and 24 more files.
