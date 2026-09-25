# Alek's Ultimate NX Edition v1.4.11

Hotfix for a 1.4.10 regression. Saves and settings carry over; the in-app
updater installs it as usual (copy `tmc_aleks_ultimate_nx.nro` to
`/switch/tmc/` as is if installing by hand).

## Fixed

- **Crash when lighting a brazier in some rooms** (1.4.10 only). The fix that
  taught the lantern to reach the Grimblade dojo braziers was too broad: it let
  the flame reach anything burnable on the tile layer Link is *not* standing
  on, and the fire that spread from there ran with a tile entry the room does
  not really have. It now crosses layers only for an unlit torch, which is the
  case the dojo needs; straw, bushes and furniture behave exactly as they did
  in 1.4.9. Thanks @mnavarretem86 for catching it the same day.

## Since 1.4.10

- Grimblade's dojo braziers can be lit (1.4.10); Palace of Winds boss room and
  Veil Falls crashes, dojo braziers made visible (1.4.9).
