# Alek's Ultimate NX Edition v1.4.14

Saves and settings carry over; the in-app updater installs it as usual (copy
`tmc_aleks_ultimate_nx.nro` to `/switch/tmc/` as is if installing by hand).

## Fixed

- **Entering a dojo could close the game.** Found while regression-testing this
  release, with the save from the Gyorg report. When a room's light is already
  off, the light manager is run directly rather than as part of the normal
  entity update, and in that path it was deleting whatever entity happened to
  run last instead of itself — or nothing at all, which is what crashed. It now
  deletes itself, and deleting "nothing" is no longer fatal. This one was not
  reported by anyone; it had been there for several versions.
- **Gyorg Pair (Palace of Winds): the final phase crashed** when hitting the
  exposed eyes, which is also the killing blow (from 1.4.12; confirmed down to
  the faulting instruction with @mnavarretem86's crash log, and reported on
  GBAtemp by @flaviometal).

## Changed

- **Hardening pass over Vaati and 24 more files.** People are reaching the end
  of the game, and the final boss had none of the protections its siblings got.
  Two of its parts walked their shared data with byte offsets that only add up
  on the GBA, which here lands in the wrong place and can overwrite a pointer
  to another part; the rest are checks for parts that failed to appear or that
  die before the code looking for them. Ported from the 3DS port, where they
  were written against real reports.

  Nobody has reported a Vaati crash and I could not reproduce one — the arenas
  behave the same before and after. These are guards put in before the reports
  arrive, not fixes for something seen. Same for most of the other 24 files.

  A handful of bosses (Mazaal, Moldorm, Gleerok) are still pending: their fixes
  depend on machinery this port does not have yet, so they need their own pass
  rather than a copied patch.
