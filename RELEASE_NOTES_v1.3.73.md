# Alek's Ultimate NX Edition v1.3.73

This hotfix repairs the third inner phase of the Mazaal fight in the Fortress
of Winds.

## Mazaal

The boss's head and inner-pillar entities used GBA-layout state without
accounting for the wider child pointer in the 64-bit Switch port. Assigning
that pointer could overwrite Mazaal's phase state, preventing the vulnerable
pillar from appearing during the third trip inside the boss and making the
fight impossible to finish.

Their state fields now retain the intended layout on both GBA and 64-bit
targets. Compile-time offset checks guard against this regression returning.

## Verification

    SHA-256  da2250af001fca8f096fdcbe14fd997203ca66eea7edcd8cc30048beb4f9e25d
             tmc_aleks_ultimate_nx_v1.3.73.nro
    Size     14223027 bytes

The clean release build passed native-layout, autosave-recovery, pause-map,
language, and all 26 v1.3 regression checks. Please report whether the third
inner phase now works correctly on real Switch hardware.
