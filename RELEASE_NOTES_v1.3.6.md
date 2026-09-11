# Alek's Ultimate NX Edition v1.3.6

This hotfix resolves the two crashes reproduced by digdat0 after v1.3.5.

## Tingle sibling crash

The new crash report exposed the exact callback and register values. The port
packs the fuser ID and dialogue ID into one 64-bit return value to reproduce two
GBA return registers. Tingle accidentally used the whole packed value as an
array index. For the reported sibling it indexed with `0x2850000003B` instead
of `0x3B`, causing the data abort.

All callers that only need the fuser ID now explicitly discard the dialogue
half before indexing save data. This also covers Din, Farore, Nayru and the
other Tingle siblings.

## Lon Lon Ranch to Lake Hylia transition

Both reports from this transition resolve to the same instruction:
`ExecuteScript` reading a null script context for Festari as Lake Hylia loads.
The Switch port stores 64-bit script pointers in a side table because they do
not fit in the original GBA entity field. Festari now reads that authoritative
table and safely waits when the room has not supplied a context. The same
protection was applied to Stockwell and at the `ExecuteScript` boundary.

Entity deletion now also clears its side-table entry so a recycled slot cannot
inherit script state from its previous owner.

The supplied `tmc.sav` is a complete 8 KB save image; this crash was not caused
by save corruption.

## Verification

    SHA-256  743587f395d8f9fb2e631f7d4c11a2045f9d154c90f583daa205db010530939d
             tmc_aleks_ultimate_nx_v1.3.6.nro
    Size     14218931 bytes

The release build passed the native-layout, autosave-recovery, pause-map and 22
v1.3 regression checks. Its AArch64 disassembly confirms a zero-extended
32-bit fuser index and a null check before the script-context dereference.
