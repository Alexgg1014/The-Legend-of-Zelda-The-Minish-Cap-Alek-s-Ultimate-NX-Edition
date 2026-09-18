# Alek's Ultimate NX Edition v1.4.0

Widescreen, a faster renderer, GBA-exact doorways, and a quality-of-life
switchboard with 360° stick movement and a 3D-Zelda-style spin attack.

## Widescreen (NORMAL display mode)

- **16:9 view (284×160)** in NORMAL mode: the camera reveals more of the room
  on both sides, the text box is centred, and the HUD anchors to the right
  edge. Rooms narrower than the view are pillarboxed, never stretched; the
  digging-cave iris and rolling room transitions run at the native width and
  restore the wide view when they finish. Toggle under SETTINGS → DISPLAY →
  SCREEN → WIDESCREEN (on by default).
- **Overlays cover the whole view**: cloud shadows, Minish Woods light rays,
  cave darkness, steam, rain and the other BG3 effects no longer stop at the
  old 240-pixel edge.
- Room-managed objects (house and festival doors) now spawn throughout the
  wide view.

## Performance

- **The software PPU renders by tile instead of by pixel** (backgrounds,
  sprites and compositing). Widescreen holds 60 FPS at the stock 1020 MHz
  clock, where 1.4.0-dev dropped to 53–56; DUAL and FLIP benefit too.

## Fixed

- **Link drawn over the door frame on house stairs / doorways and the Castor
  Wilds swamp sink.** Two causes, both fixed: the PPU now implements the GBA's
  OBJ priority propagation (a transparent sprite of better priority lifts the
  sprite under it — the mechanism the game's `Object70` relies on, which
  earlier ports approximated), and the port's own crash-context snapshot was
  calling an engine routine that rewrote Link's priority every frame.
- A player item with an out-of-range id is dropped and logged instead of
  jumping through garbage (reported once on an emulator after an ocarina warp).
- Upstream 999sian/tmc post-0.9.3 fixes: door spawning at the wide view's
  edges, rolling transitions in wide mode, Minish Woods rays across the view,
  sitting NPCs' dialogue condition on 64-bit.

## Quality of life (SETTINGS → GAMEPLAY → QUALITY OF LIFE)

Every entry is a toggle and persists in `config.json` (`reborn_mask`).

- **360° stick movement** — Link walks in 32 directions with the left stick
  (the D-pad keeps its 8). On by default.
- **Spin attack by stick circle** — roll the stick through a circle and press
  B (before or right after finishing it) for an instant spin attack, 3D-Zelda
  style. Needs the spin attack scroll; 1.5 s cooldown. On by default.
- **Roll attack button** — one button rolls and thrusts with your best sword
  (needs the roll attack scroll). Bind it under CONTROLS → ROLL ATTACK
  (default RSTICK); it refuses to fire while it shares a button with TALK TO
  EZLO.
- **Shells cap 9999**, **no Ezlo hint right after loading a save**,
  **figurine odds never below 20 %** — on by default.
- **Skip Ezlo tutorials**, **Hero Mode (2× damage)** — off by default.

The first four gameplay ones are ported from The Minish Cap Reborn
(Admentus64, GPL-3.0) by way of Project Picori; see THIRD_PARTY_NOTICES.md.

## Screen filters (SETTINGS → DISPLAY → RENDERING → SCREEN FILTER)

- **SCANLINES**, **SCANLINES SOFT** and **LCD GRID**, drawn as one
  alpha-blended overlay on the GPU over the game rectangle — zero CPU cost,
  no FPS impact, in every display mode. The pattern follows GBA pixel
  boundaries at any scale. (The CPU CRT approximations remain debug-only:
  they cannot hold 60 FPS on the console.)

## Settings menu

- Long pages are paged (MORE 1-2 row) instead of squeezed; SYSTEM always
  shows every row. Long values no longer get cut off.

## Notes

- `assets/` on the SD card is not refreshed by an NRO update. If something
  looks wrong that is not on this list, rename `switch/tmc/assets` and relaunch.
- Saves are unchanged from 1.3.9 (stream order, mGBA-compatible).

## Verification

    SHA-256  5ddf2bc78a1a998ae9f1201e989a10feb210320312045aa3275df31f922460d5
             tmc_aleks_ultimate_nx_v1.4.0.nro (14218931 bytes)
    Build ID d7ed6e645dc3d4bd18af0ef1801e8a8d590d0a12
