# Alek's Ultimate NX Edition v1.4.9

Hotfix. Saves and settings carry over; the in-app updater installs it as
usual (copy `tmc_aleks_ultimate_nx.nro` to `/switch/tmc/` as is if installing
by hand).

## Fixed

- **Palace of Winds: entering the boss room closed the game.** The boss sets
  up its parts over two frames, but a per-frame routine already reached for
  the mouth and tail on the first one, when they do not exist yet. The GBA
  shrugs that off; the Switch did not. The phase transitions of the fight had
  the same problem when a Gyorg male finished dying, and the code that retires
  a dead male was overwriting part of the boss data on 64-bit. Thanks
  @rdgrsrz for the report.
- **Veil Falls Top: the game closed on the way to the tornado, and walking
  west past Biggoron crashed too.** Biggoron's backdrop was read from two
  buffers that sit next to each other on the GBA but not here, so one read
  landed outside memory entirely and the other quietly filled the wrong place
  (which is why his backdrop was missing). Both routed properly, and the slice
  is clamped so walking off his west side is harmless. Thanks @flaviometal for
  the report and the screenshot.
- **Grimblade's dojo: the two braziers were invisible** while still blocking
  Link. When two background layers share a priority the GBA draws the
  lower-numbered one on top; the renderer's ordering could swap them, hiding
  one layer behind the other. Thanks @mnavarretem86 for the report, the save
  and for pointing straight at the layer sorting.

## Since 1.4.8

- Temple of Droplets waterfall lily pad room (1.4.8) and boss door crash
  (1.4.7); sunbeam, statue reward and beetle mandibles (1.4.6).
