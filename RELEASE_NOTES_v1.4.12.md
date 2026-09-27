# Alek's Ultimate NX Edition v1.4.12

Hotfix. Saves and settings carry over; the in-app updater installs it as
usual (copy `tmc_aleks_ultimate_nx.nro` to `/switch/tmc/` as is if installing
by hand).

## Fixed

- **Gyorg Pair (Palace of Winds): the game crashed in the final phase**, when
  hitting the exposed eyes, which also made it impossible to finish the fight.
  Each Gyorg male removes itself from the boss's shared data the moment it
  finishes dying, so by the last phase there is none left — and the eye code
  still went looking for one and read through the empty slot. It now treats "no
  male left" as what it means, defeated. The two eye entities also got the same
  missing-parent guard their siblings already had. Thanks @mnavarretem86 for
  the report and the crash log (the fault address pointed straight at it), and
  @flaviometal on GBAtemp for reporting a crash at the end of the same fight.

## Since 1.4.11

- Brazier crash hotfix (1.4.11), Grimblade dojo braziers can be lit (1.4.10),
  Palace of Winds boss room and Veil Falls crashes (1.4.9).
