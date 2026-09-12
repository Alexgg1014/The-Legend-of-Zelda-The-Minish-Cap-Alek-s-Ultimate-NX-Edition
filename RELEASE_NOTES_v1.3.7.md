# Alek's Ultimate NX Edition v1.3.7

This hotfix repairs the Cave of Flames boss-door room reported after v1.3.6.

## Cave of Flames collision

Link could walk over magma and pass through the movable stone barriers after
crossing specific cells in the room. The Gust Jar and Cane of Pacci also
appeared to stop interacting with the room at the same point.

The PC/Switch implementation of `CheckOnLayerTransition` had reversed a branch
from the original GBA routine. The incorrect condition moved Link to the upper
logical map layer while the lava, special collision tiles, and item interaction
geometry stayed on the lower layer. The comparison now matches the original
ARM instruction, keeping Link and the room geometry on the same layer.

## Verification

    SHA-256  36caae21b7064b57a33591155b976d6211fb167943a5c555c0526bf141a62781
             tmc_aleks_ultimate_nx_v1.3.7.nro
    Size     14218931 bytes

The release build passed native-layout, autosave-recovery, pause-map, language,
and 23 v1.3 regression checks. The new regression check requires the native
layer-transition comparison to retain the branch direction used by the GBA
assembly.
