# Alek's Ultimate NX Edition v1.4.7

Hotfix. Saves and settings carry over; the in-app updater installs it as
usual (copy `tmc_aleks_ultimate_nx.nro` to `/switch/tmc/` as is if installing
by hand).

## Fixed

- **Temple of Droplets: crash when opening the boss door.** The room behind
  it spawns the frozen Big Octorok, and its leg parts read the mouth part's
  health one frame before the mouth exists. On the GBA that stray read is
  harmless; on the Switch it was a Data Abort the moment the room loaded.
  Thanks @flaviometal for the report. The thaw cutscene, the slide into the
  boss room and the fight were run end to end after the fix.
- **Big Octorok fight:** the same class of read guarded in the boss itself
  (camera target, death explosion, tail and leg updates), mirroring the fixes
  the 3DS port carries.

## Since 1.4.6

- Temple of Droplets sunbeam, Dark Hyrule Castle statue reward, beetle
  mandibles crash (1.4.6); credits roll and soft reset (1.4.5); drag-and-drop
  equipment rings (1.4.4).
