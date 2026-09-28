# Alek's Ultimate NX Edition v1.4.17

Saves and settings carry over; the in-app updater installs it as usual (copy
`tmc_aleks_ultimate_nx.nro` to `/switch/tmc/` as is if installing by hand).

## Fixed

- **Assigning an item to X / Y / ZL / ZR from the pause screen also changed
  A and B.** The new 1.4.16 shortcut did its job and then equipped the same
  item a second time: a soft slot holding an item forces the GBA's B button so
  the engine can fire it, and inside the inventory that press is the vanilla
  "equip to B". With the cursor on the sword the two boxes even swapped, which
  is why it looked like X wrote A and Y wrote B. The press now assigns and
  nothing else, and holding the button while you close the menu no longer uses
  the item on the way out. Reported by @Alexgg1014 the same day 1.4.16 went up.

  Measured with a probe inside the inventory handler: before the fix the frame
  after the press carried `newKeys=0002` (B) and the equipped pair went from
  `A=sword B=lantern` to `A=lantern B=sword`; after it, all four buttons show
  `newKeys=0000` with the equipped pair untouched, while A and B still equip
  normally and a slot held during play still fires its item.

## Since 1.4.16

- Assign items to X / Y / ZL / ZR straight from the game's own inventory
  (issue #13), the four-cannon puzzle, the dojo crash and the Gyorg Pair final
  phase.
