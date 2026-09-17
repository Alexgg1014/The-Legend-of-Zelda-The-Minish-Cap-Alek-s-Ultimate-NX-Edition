# Alek's Ultimate NX Edition v1.3.9

Community reports from GBAtemp, a sweep of engine fixes from the upstream PC
port (999sian/tmc v0.9.3), and a better controller experience for the
second screen in Normal display mode.

## Fixed (reports)

- **Library bookshelf Minish could not be talked to.** The Town Minish that
  appears at the foot of the library bookshelf once the book quest starts was
  the only entity in the game defined in C instead of read from the ROM, and
  its flags byte was transcribed without the "scripted" bit. The NPC never
  attached its dialogue script and never registered as interactable — R
  showed *Roll* instead of *Talk*. The two counter librarians were never
  affected.
- **Link half-hidden walking into house stairs / doorways (and during the
  Castor Wilds swamp sink).** On GBA Link drops behind the background and a
  separate head-overlay sprite is drawn on top; the port never rendered that
  overlay, leaving a thin band of Link visible. He now stays fully visible
  (same approach as upstream and the 3DS build).
- **Second screen in Normal display mode vanished on Quest / Map / Items.**
  The overlay was tied to the Settings tab; it is now latched open by Minus
  independently of the tab.
- **Saves are interchangeable with mGBA / VBA-M / cart dumps.** Every 8-byte
  EEPROM block sits in memory reversed relative to the serial stream that
  emulators store, and the port wrote its memory image verbatim. `tmc.sav`
  is now written in stream order and both orders are auto-detected on load
  (existing saves keep working). Copy `tmc.sav` to `<rom>.sav` or back with no
  conversion. **A 1.3.8 or earlier NRO will not read a save written by 1.3.9.**

## Second screen (Normal mode) controls

- Quest / Map / Items: D-pad moves in all four directions by position; **A**
  activates. Items: **A** equips to the A slot, **B** to the B slot.
- **L / R** (your configured shoulder buttons) cycle Quest → Map → Items →
  Settings.
- Map: **A** on the map opens the region Link is standing in; ZOOM still
  toggles the close / whole-Hyrule view; Back / **B** returns.

## Upstream engine fixes ported (verified by content against this tree)

Entity update order now matches GBA (fixes entities skipped or updated twice
after a self-delete, and enemy-target leaks); BIOS-accurate `ObjAffineSet`;
full 16-byte ice-velocity table; Great Fairy light no longer stuck on her
face; Wind Tribe Tower 2F Gregal softlock; Hyrule Castle Garden knights
frozen; OBJ palette slot 15 released again (NPCs drawn with wrong colours in
busy rooms); nine per-room entity lists that were empty stubs now load from
ROM (Mayor's house, Mayor's cabin Minish path, all Happy Hearth Inn 2F oracle
lists — the Din/Nayru/Farore house sidequest); Vaati Reborn phase/eye/projectile
hardening; ChuChu boss, Gyorg tails/eyes, tennis-ball and mushroom entity-pool
guards; item-get cutscene retries instead of losing the pickup; `{Player}`
name in figurine screens; Goron Kinstone script callbacks; several
out-of-bounds table reads (gfx groups, text banks, room properties, area exit
lists); GFX slot allocation and compaction guards; per-scanline HDMA stopped
between rooms; and more (see CHANGELOG).

## Verification

    SHA-256  347a72490e7cc85177ed787a3056bddad3e9959ec2b2f9c03df1b70a83c67c4b
             tmc_aleks_ultimate_nx_v1.3.9.nro
    Size     14198451 bytes

The clean release build passed native-layout, autosave-recovery, pause-map,
language, and all 26 v1.3 regression checks. The library, stairs, second-screen
and save-order changes were verified on Eden and the desktop harness.
