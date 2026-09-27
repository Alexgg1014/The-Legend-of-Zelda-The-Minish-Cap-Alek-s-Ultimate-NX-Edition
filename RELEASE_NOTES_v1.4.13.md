# Alek's Ultimate NX Edition v1.4.13

Preventive hardening for the endgame. Saves and settings carry over; the in-app
updater installs it as usual (copy `tmc_aleks_ultimate_nx.nro` to
`/switch/tmc/` as is if installing by hand).

## Changed

- **Vaati (the three final arenas): a hardening pass.** People are reaching the
  end of the game now, and the final boss had none of the protections its
  siblings got. Two of its parts walked their shared data with hardcoded byte
  offsets that only add up on the GBA, which on this port lands in the wrong
  place and can overwrite a pointer to another part; the rest are checks for
  parts that failed to appear or that die before the code looking for them.
  Ported from the 3DS port, where they were written against real reports.

  To be clear about what this is: nobody has reported a Vaati crash here and I
  could not reproduce one — the arenas run the same before and after. These are
  guards put in before the reports arrive, not fixes for something seen.

## Fixed in 1.4.12 (included here)

- The Gyorg Pair fight crashed in its final phase, when hitting the exposed
  eyes. Verified down to the faulting instruction with the reporter's crash log.
