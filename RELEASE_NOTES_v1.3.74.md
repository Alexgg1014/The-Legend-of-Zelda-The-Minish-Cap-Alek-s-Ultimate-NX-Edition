# Alek's Ultimate NX Edition v1.3.74

This hotfix restores the Minish-sized doors of Hyrule Town and repairs three
other objects that talked to their manager through the same broken shortcut.

## Hyrule Town Minish doors (#9)

The small doors Minish Link uses to enter and leave houses in Hyrule Town
are spawned by a manager that tracks them in a bitfield. Each door checked
that bitfield through an Entity field that only overlapped it in the GBA
memory layout. On the 64-bit Switch port the manager structure is wider, so
the check read pointer bits instead, failed, and every door deleted itself
one frame after appearing — most visibly the moment Link stepped back
outside.

The doors now read the manager's real bitfield and persist as long as they
are on screen.

## Same fix, four more objects

The same GBA-only field overlap was used by four other manager-spawned
objects. None of them could report back to their manager on Switch:

- **House signs** never cleared their spawn bit after scrolling off screen,
  so a sign could fail to come back once it left the view.
- **Goron merchant kinstone pieces** were never marked as sold, so the
  purchase was not recorded.
- **Pushable furniture puzzles** never registered a piece as in place, so the
  puzzle could not complete.
- **Angry statues** never reported themselves destroyed, so a room could
  not register the full set as cleared.

All four now write the manager's real field.

## Verification

    SHA-256  283a242a2e9f3970d6e9c18b746e697aae992be0a1df48473b5bb8020371458b
             tmc_aleks_ultimate_nx_v1.3.74.nro
    Size     14223027 bytes

The clean release build passed native-layout, autosave-recovery, pause-map,
language, and all 26 v1.3 regression checks. Please report whether the
Hyrule Town doors now stay in place on real Switch hardware.
