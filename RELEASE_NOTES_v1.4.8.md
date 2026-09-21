# Alek's Ultimate NX Edition v1.4.8

Hotfix. Saves and settings carry over; the in-app updater installs it as
usual (copy `tmc_aleks_ultimate_nx.nro` to `/switch/tmc/` as is if installing
by hand).

## Fixed

- **Temple of Droplets: the lily pad vanished at the bottom of the waterfall
  and Link could not go on.** The pad is meant to dissolve there: Link drops
  through the pit into the room below, where a second pad is waiting. Since
  1.4.3 that room loaded no entities at all (no pad, no chest, no bollards),
  so Link landed in deep water. Its entity list opens with an unused entry
  type that the 1.4.3 entity-list guard rejected, stopping the whole list.
  Thanks @flaviometal for the report. Verified end to end: pad down the fall,
  pit, landing on the B2 pad.

## Since 1.4.7

- Temple of Droplets boss door crash (1.4.7); sunbeam, Dark Hyrule Castle
  statue reward, beetle mandibles crash (1.4.6).
