# Alek's Ultimate NX Edition v1.3.71

This hotfix repairs the Castor Wilds Eyegore bow puzzle.

## Eyegore statues

The one-eyed Eyegore statues could stay closed permanently and reject every
arrow. A 64-bit layout mismatch made them mistake regular entity metadata for
their puzzle flag, so collision was disabled before the eye could open.

The port now restores the original GBA field alignment. Eyegores open their
eyes, bow shots register on the eye, and the Castor Wilds puzzle can be
completed normally.

## Verification

    SHA-256  c11aab7d878d72e57cd61116b2570e2263abf5bb5454f1d8ff4c3ed994eb2339
             tmc_aleks_ultimate_nx_v1.3.71.nro
    Size     14218931 bytes

The release build passed language validation and all 24 v1.3 regression
checks, including a new Eyegore layout check that preserves the required
64-bit padding.
