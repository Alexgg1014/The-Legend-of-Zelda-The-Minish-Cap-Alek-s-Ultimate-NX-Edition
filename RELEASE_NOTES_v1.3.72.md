# Alek's Ultimate NX Edition v1.3.72

This release repairs the Fortress of Winds FloorMaster encounter and adds an
in-game update notification.

## Fortress of Winds

WallMasters and FloorMasters used GBA-layout AI state without accounting for a
wider pointer in the 64-bit port. Their target and movement state could be
corrupted, making the FloorMasters in the Minish portal room disregard Link.

Their fields now match the original layout, restoring normal detection and
pursuit behavior.

## Update notification

When the background update check finds a newer verified release, the game now
shows a non-blocking in-game popup for ten seconds. It displays the target
version and up to three short changelog entries from the release manifest.
Offline or failed checks remain silent.

## Verification

    SHA-256  2735975248038b1882791c7691ea2ea3799c23097159e8b58efb337ddd041d7e
             tmc_aleks_ultimate_nx_v1.3.72.nro
    Size     14223027 bytes

The release build passed native-layout, autosave-recovery, pause-map, language,
and all 26 v1.3 regression checks.
