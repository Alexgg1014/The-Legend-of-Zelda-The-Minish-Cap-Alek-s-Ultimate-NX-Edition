# Alek's Ultimate NX Edition v1.4.10

Hotfix. Saves and settings carry over; the in-app updater installs it as
usual (copy `tmc_aleks_ultimate_nx.nro` to `/switch/tmc/` as is if installing
by hand).

## Fixed

- **Grimblade's dojo: the braziers would not light.** 1.4.9 made them visible
  again, but the Flame Lantern still did nothing to them, so the Sword Beam
  training could not be finished. The braziers sit on the upper tile layer
  while Link walks on the lower one, and both halves of the lantern code only
  ever looked at the layer Link is standing on: the check that starts the
  burning animation, and the one that actually sets the tile on fire. Both now
  fall back to the other layer. Thanks @mnavarretem86 for the report, the save
  and the follow-up testing — verified with that save: one brazier, then both,
  and the room's darkness lifts as it should.

## Since 1.4.9

- Palace of Winds boss room and Veil Falls Top crashes, dojo braziers made
  visible (1.4.9); Temple of Droplets waterfall lily pad (1.4.8) and boss door
  crash (1.4.7).
