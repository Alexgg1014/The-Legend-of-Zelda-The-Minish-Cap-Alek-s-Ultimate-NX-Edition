# Alek's Ultimate NX Edition v1.4.5

The ending works now. Saves and settings carry over; the in-app updater
installs it as usual.

From this release the download is named `tmc_aleks_ultimate_nx.nro` — copy it
to `/switch/tmc/` as is. That is the name the in-app updater has always
expected, so a fresh install updates itself from now on without renaming.

## Fixed

- **Credits never rolled after beating Vaati.** The outro's "Roll Credits"
  script call was missing from the port's function table, so it was skipped
  and the game just gave control back after the final cutscene. It now rolls
  the staff credits like the original.
- **Soft reset closed the app.** The end of the credits, "game over → don't
  continue" and the Start+Select+A+B combo all quit to the home menu. They now
  restart to the title screen like the GBA does; your save is untouched.

Both were found by comparing against EstebanPdN's zelda-tmc-3ds v2.0, which
fixed them on the 3DS side after our fork. Thanks Esteban.

## Since 1.4.4

- Drag-and-drop equipment rings on the second screen (1.4.4), Lake Hylia and
  cat crashes (1.4.3), updater (1.4.2), cat-girl dialogue / doors / Pegasus
  steering (1.4.1).
