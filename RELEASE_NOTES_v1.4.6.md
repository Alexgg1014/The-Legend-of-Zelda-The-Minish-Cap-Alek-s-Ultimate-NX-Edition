# Alek's Ultimate NX Edition v1.4.6

Hotfix. Saves and settings carry over; the in-app updater installs it as
usual (copy `tmc_aleks_ultimate_nx.nro` to `/switch/tmc/` as is if installing
by hand).

## Fixed

- **Temple of Droplets: the sunbeam is back.** The temple's light manager was
  reading its data at the wrong offset on 64-bit, so the beam never rendered
  in any room and the light-gated ice puzzles ran on a zero flag. Thanks
  @digdat0 for the report and the save. If you're already inside the temple,
  the light on the ice appears once you've opened the corresponding hole above
  it, as in the original.
- **Dark Hyrule Castle: statue-room reward.** Same defect: destroying all four
  Angry Statues never set the reward flag.
- **Beetle mandibles crash** when the beetle dies in the same frame.

## Since 1.4.5

- Credits roll after Vaati and soft reset restarts to the title (1.4.5),
  drag-and-drop equipment rings (1.4.4), Lake Hylia / cat crashes (1.4.3).
