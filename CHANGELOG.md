# Changelog

## v1.3.72 — Fortress of Winds and update notification

### Fixed

- **WallMasters and FloorMasters now detect and pursue Link correctly.** Their
  64-bit AI state layout now matches the original game, repairing the
  FloorMaster encounter in the Fortress of Winds Minish portal room.

### Added

- **Update popup.** When the game finds a newer release, it now shows an
  in-game notification with the target version and a short summary of the
  published changelog. Offline checks remain silent.

## v1.3.71 — Castor Wilds Eyegore hotfix

### Fixed

- **Eyegore statues in Castor Wilds kept their eyes closed and rejected bow
  shots.** A 64-bit structure-layout mismatch made the statues read entity
  metadata as their puzzle-completion flag, disabling their collision before
  the eye could open. Their fields now align with the original GBA layout, so
  arrows hit correctly and the bow puzzle can be completed.

## v1.3.7 — Cave of Flames collision hotfix

### Fixed

- **Link could walk on magma and pass through the movable stone barriers in
  the Cave of Flames boss-door room.** The native port had inverted one branch
  from the original GBA layer-transition routine. Crossing the affected cells
  incorrectly moved Link to the upper logical map layer while the room's lava
  and solid geometry remained on the lower layer.

- **The Gust Jar and Cane of Pacci appeared to stop working in the same area.**
  Their effects now stay on the same collision layer as Link and the room
  geometry, restoring item interactions after crossing those cells.

## v1.3.6 — Tingle and Lake Hylia hotfix

### Fixed

- **Talking to a Tingle sibling crashed in Hyrule Field.** The port's
  `GetFuserId` compatibility function carries two GBA return registers in one
  64-bit value: the fuser ID in the low word and the dialogue ID in the high
  word. Tingle used the full packed value as an index into the save, producing
  an address far outside `fuserProgress`. Scalar callers now explicitly take
  the low word; all affected NPC callers were migrated and guarded by a
  regression check.

- **The Lon Lon Ranch to Lake Hylia transition could freeze on white, then
  crash after reopening the game.** Both hardware reports faulted in
  `ExecuteScript` for Festari with a null context. On the 64-bit port the
  authoritative script pointer lives in a per-entity side table. Festari and
  Stockwell now read that table and skip script work when no context exists;
  `ExecuteScript` also rejects a null context at its boundary.

- **Recycled entity slots could retain a dead script context.** Entity deletion
  now clears the corresponding side-table entry before the slot is reused.

## v1.3.5 — the v1.3.3 save freeze

### Fixed

- **Creating a save file froze the game.** v1.3.3's atomic swap was performed
  per 8-byte EEPROM block: create tmc.sav.tmp, write 8 KB, close, remove
  tmc.sav, rename. Four SD metadata operations per block, ~160 blocks for a save
  and several hundred when the engine formats the EEPROM for a new game -- a
  measured 4096 filesystem operations where there should be 3. On hardware that
  is a freeze of minutes, and force-closing out of it left a stranded
  tmc.sav.tmp with no tmc.sav, because the remove had already run. Blocks now
  only mark the image dirty; Port_Save_Tick commits at most one swap per frame,
  driven from port_bios.c beside the autosave tick. The swap itself stays: a
  fragment on the card is how saves were lost to begin with. A complete stranded
  tmc.sav.tmp is adopted on load, so nobody bitten by v1.3.3 loses progress.

  Also added: tools/save_selftest, which compiles the real port_save.c on the
  host with fopen/remove/rename redirected to counting wrappers. It asserts the
  operation count, not just the behaviour -- the property that would have caught
  this before it shipped.

## v1.3.4 — screen-transition crash, and the updater brought back

### Fixed

- **Null owner crash on a screen transition.** DarkNutSwordSlash dereferenced
  `this->parent` in DarkNutSwordSlash_Init before the existing
  `parent == NULL -> DeleteThisEntity()` check a few lines below it, so a slash
  projectile ticked by ProjectileUpdate during GameMain_ChangeRoom -- respawned
  as a room entity with no owner yet -- faulted on `far = 0x12`, Entity.type's
  native offset. On GBA that read lands in the BIOS and the delete cleans up
  regardless. The check is hoisted above the dereference; DeleteThisEntity does
  not return, so the end state is retail's. Reported by digdat0 on the Lon Lon
  Ranch transition, resolved from the Atmosphere module base plus the archived
  .elf: ProjectileUpdate -> DarkNutSwordSlash.

- **A manifest with four changelog lines bricked the in-game updater.**
  JsonChangelog returned 0 on the entry past its cap of three, which fails
  TmcUpdate_ParseManifest outright, so the game reported "Update manifest
  rejected". v1.3.2 was the first release to ship four lines; the updater has
  been dead since. The published manifest was trimmed by hand to restore every
  install already out there -- a code fix cannot reach a copy whose updater is
  broken. Here the cap rises to 8 and an overflow is skipped instead of fatal.

- **The v1.3.3 crash-report anchor named the wrong process.** A crash is stored
  as crash_pending.bin and rendered by the next boot, so printing the anchor at
  write time described that later process, not the one holding the registers.
  digdat0's reports proved it: anchor 0xAB962EF0 minus its .elf address gave the
  base of the process that WROTE the log. It is now sampled at capture time and
  carried in the blob (context version 3).

### Diagnostics

- Crash reports name the script command being dispatched. A ScriptCommand_*
  handler that faults leaves no frame of its own -- digdat0's Tingle crash
  resolved to ExecuteScript and stopped, with 0x8a candidates and no way to
  narrow it. Recorded in the crash context rather than a breadcrumb, since
  scripts step every frame and would flush the ring.

### Still open

- Talking to a Tingle sibling in Hyrule Field crashes inside a script command
  handler. Not fixed; the next report will name the command.

## v1.3.3 — Lake Hylia crash, and achievements that really work offline

Hotfix for a reproducible crash entering Lake Hylia while heading to Syrup's
Hut for the Wake-Up Mushroom, plus the offline achievement cache missing the
one request that carries the set.

### Fixed

- **Out-of-range NPC sprite-table lookup.** `LoadExtraSpriteData(this,
  table[this->type])` indexed a 21-entry table of pointers with a `type` of
  64. The read ran 43 slots past the end into the font colour buffer BSS
  places behind it and produced a fake pointer, which the sprite loader then
  dereferenced — Data Abort. On GBA the same index reads a ROM word: wrong
  sprite, no crash. Both pointer tables of that shape (townspeople, children)
  are now bounded, and `LoadExtraSpriteData` treats a null table as "no extra
  parts" instead of faulting. A build check ties each bound to the table's own
  size. Why the NPC carries type 64 is still open; this only makes it
  survivable.

- **Offline achievements never cached the set.** `IsCacheable()` in
  port_ra_offline.c listed the endpoints an offline boot may replay from the SD
  card, and the game-data step was listed under its old name, `patch`. rcheevos
  12 fetches the set through `achievementsets` (rc_client_begin_load_game ->
  rc_api_init_fetch_game_sets_request_hosted) and never issues `patch` at all,
  so the cache stored login and startsession but not the payload the gallery is
  built from: an offline boot logged in from cache and then showed NO
  ACHIEVEMENT SET LOADED. Silent since v1.3.0. One online boot after updating
  writes the set; offline boots then work. A build check resolves the three
  init functions rc_client calls back to their endpoint strings and fails if any
  is not cacheable, so a library bump cannot re-introduce this.

- **Unlocked achievements kept their grey badge.** RetroAchievements serves two
  images per badge id (`155791.png` and `155791_lock.png`), but all three badge
  caches — the resident pixel cache, the gallery prefetch, and the unlock-toast
  download — keyed on the id alone and check the cache before the network. The
  first image to arrive won permanently, so an achievement whose badge had been
  prefetched while locked kept the grey locked art after unlocking: green title,
  grey picture. Cache entries are now keyed by state through one shared
  `Port_RA_Gallery_BadgeKey`, with the unlocked art deliberately not landing on
  the old path, which already holds locked art on installs that ran an earlier
  build. Old entries are never read again; deleting `switch/tmc/ra_badges/` is
  optional housekeeping.

- **A save could be lost by closing the game while it wrote.** The engine
  commits a save one 8-byte EEPROM block at a time, ~160 times per file, and
  port_save.c flushed after every block with `fopen("tmc.sav", "wb")` -- which
  truncates the file to zero before rewriting all 8 KB. Killing the process
  inside any of those windows (closing homebrew from the HOME menu does it) left
  tmc.sav short; the loader ignored fread's return value, the engine saw a
  broken header and formatted, and all three save files were gone. The flush now
  writes a temp image and swaps it in, so the live file is only ever replaced by
  a complete one; the loader refuses anything that is not a whole EEPROM image
  and falls back to a session-start `tmc.sav.bak`. Reported and reproduced
  2026-09-08.

- **Achievements earned offline never reached the server.** The unlock was
  written to `ra_unlock_queue.bin` correctly, but the only caller of the drain
  was RC_CLIENT_EVENT_RECONNECTED -- an event rc_client raises only after a
  disconnect *in the same session*. An unlock earned offline in a run that then
  exited was never looked at again, which is precisely the case the on-disk
  queue exists for. The queue is now drained whenever a game finishes loading,
  the "every boot" contract its own header always stated.

### Diagnostics

- Crash reports now print an `anchor` line: the runtime address of a known
  function, so the load base is `anchor - (its address in the .elf)` and `pc`
  resolves to a function name from the report alone. Previously that base lived
  only in Atmosphere's own crash report -- the file that never arrives with the
  bug report. A Kinstone fusion also drops a `FUSE_BEGIN` breadcrumb naming the
  fuser and the candidate slot.

## v1.3.1 — Cave of Flames lava platforms, properly this time

Hotfix for two reports against v1.3.0: platforms still missing on B2, and in
one room the Gust Jar and Cane of Pacci stopping too.

### Fixed

- **Four more under-indexed data symbols.** v1.3.0 fixed one truncated
  structure and assumed the pattern was "symbol opens with a pointer word".
  The real rule is that a symbol's extent runs to the next symbol, and the
  index only ever records its first `.incbin` fragment. Under that rule there
  are five affected symbols, not two — including Cave of Flames B2's platform
  group (176 bytes recorded as 32, ten platforms recorded as two) and two
  outside the dungeon entirely, in Hyrule Town and near Lon Lon Ranch. The
  build now re-derives the table from the decompilation and fails if a sixth
  appears.

- **Items no longer die alongside the platforms.** Entities share one 72-slot
  pool, so a spawner running off the end of a truncated buffer consumed the
  pool and left the Gust Jar and Cane of Pacci with nowhere to spawn their
  effects. v1.3.0's runaway cap was 64 — nearly the whole pool, so it stopped
  the bad reads without saving the items. It is now derived from the pool size
  (9 of 72).

## v1.3.0 — Português (Brasil), Cave of Flames, offline achievements

### Added

- **The whole game in Brazilian Portuguese.** 2910 messages across 80 banks —
  dialogue, signs, item text — not just the menus. Brazilian, not European:
  *salvar* not *guardar*, *você* not *tu*, *tela* not *ecrã*. Runs on the USA
  ROM; language and ROM region are independent. Accented glyphs are composed
  from the existing font at runtime so they match the rest of the text.

- **Offline RetroAchievements.** The session is cached so the achievement set
  loads with no network, and unlocks earned offline are written to the SD card
  and sent on reconnect with their original unlock time. Softcore only, the
  condition RetroAchievements attaches to this pattern. This is the least
  tested code in the release: offline earning and sync were not verified on
  hardware before publishing.

### Fixed

- **Cave of Flames B1: the lava platform never appeared**, leaving a crossing
  that could not be made. The asset index is generated from `.incbin`
  directives, so a data symbol beginning with a pointer word is recorded four
  bytes past its true start — that room's platform data was sized as 4 bytes
  instead of 32, and the spawner walked off the end of the buffer. Both
  affected symbols now carry their true extent, and the game falls back to the
  ROM whenever a room property is shorter than the structure it holds, so the
  fix reaches existing installs without regenerating assets. Logged to
  `assetfix.log`.

- **The second screen no longer spoils the overworld.** Undiscovered regions
  are covered and cannot be tapped into, matching the in-game map.

- Minish Path leaves and parallax at the top of vertical rooms.
- Sword charge graphics facing east; arrow collision in all four directions.
- Gleerok's fire and neck segment bounds.
- Pullable mushroom child lifetime.
- Delayed entity loader spawn-bit ordering.
- A late-game Vaati progression flag that could leave a save unable to advance.
- Graphics table bounds hardened against out-of-range indices.

## v1.0.2 — diagnostics in release builds

Makes released builds self-diagnosing, so a bug report can carry evidence
instead of a description. No gameplay changes.

### Added

- **Boot log in release builds.** Released builds now write
  `/switch/tmc/tmc.log` (build identity, asset bootstrap, ROM symbol
  resolution, network bring-up, and any failure along the way) and
  `/switch/tmc/startup.log` (per-phase boot timings). Previously both were
  compiled out of release builds, so a player's build could produce no
  evidence at all.

  This is close to free: every per-frame gameplay trace is still discarded
  before the game loop starts, exactly as before, so nothing is written to the
  SD card during play. `tmc.log` self-trims at 2 MiB.

- **Documented crash reporting.** Crash reports were already produced by
  release builds — that was never a released feature people knew about. If the
  game crashes it writes `/switch/tmc/crashlogs/`, alongside the system's own
  report in `/atmosphere/crash_reports/`.

- **[Reporting a bug](README.md#reporting-a-bug)** section in the README, with
  the exact file paths to attach.

### Notes

- If you are on v1.0.1 nothing is broken; this release only makes future
  problems easier to diagnose. Updating is still recommended, because any bug
  you hit afterwards will come with logs attached.

## v1.0.1 — Mt. Crenel hotfix

Fixes a single rendering bug reported within hours of v1.0.0. Everything from
v1.0.0 is preserved; no save or configuration migration is required.

**This is also the first update deliverable through the in-app updater** — if
you are on v1.0.0, the game will offer it to you under CONFIG → SYSTEM.

### Fixed

- **Black background at the top of Mt. Crenel.** At the mountaintop the
  weather-change manager cross-fades the background palettes from a 26-palette
  block (`gPalette_549`). The port allocated that block but the code that was
  supposed to fill it from the ROM's palette data was never written, so it
  stayed all zeros — and a palette of zeros is pure black. The cross-fade
  blended the background to black while Link, enemies and the HUD (sprite
  palettes, untouched by the fade) rendered normally. The block is now
  populated from the ROM at load, verified against the USA ROM's actual
  palette data.

## v1.0.0 — the first stable release

The first release this project considers stable. It adds an in-app updater, a
live position marker on the second-screen map, and a rebuilt autosave store,
and it corrects the project's attribution of its own lineage.

Everything from v0.1.2 is preserved. No save or configuration migration is
required: existing `tmc.sav`, autosave data and `config.json` are used as-is.

### Added

- **In-app updater.** The game can now check for, download and install its own
  updates from the second screen, under SYSTEM. It is deliberately conservative:

  - Update metadata comes from a project-controlled manifest with a **closed
    schema** — any unknown key, duplicate key, wrong channel, non-HTTPS URL or
    malformed field causes the whole manifest to be rejected. It is not a
    GitHub release scrape.
  - The expected **size and SHA-256 are pinned before a single byte** of the
    new build is fetched, and the download is verified against them.
  - Downloads are HTTPS-only, with HTTPS-only redirects and certificate
    verification.
  - Installing **creates and verifies a backup** of the current build
    (`tmc_aleks_ultimate_nx.bak`) before the installed game is touched. If any
    step fails, the backup is restored automatically and the installed game is
    left exactly as it was.
  - Progress is journalled, so an interrupted install is recoverable rather
    than leaving a half-written game.

  After installing, close the game and open it again to run the new version.

- **Link's position marker on the second-screen MAP tab.** The map now shows
  where you actually are, live.

### Changed

- **Rebuilt autosave storage.** Autosave now uses a checksummed three-slot ring
  with an explicit discovery rule, never renames a file into place, and
  migrates older autosave data forward on first run. The goal is that a crash
  or a power loss mid-write can never leave you without a readable autosave.
- **Corrected lineage and attribution.** Earlier releases credited
  [EstebanPdN/zelda-tmc-3ds](https://github.com/EstebanPdN/zelda-tmc-3ds) only
  as a *reference*. That was wrong. This edition's core actually **descends
  from that project** — the Switch tree was branched from its native port at
  commit `afdde1b7` (2026-05-10), where 742 of 768 core source blobs match.
  It is now credited as a direct code ancestor. See
  [CREDITS.md](CREDITS.md#on-the-estebanpdn-fork-point).

### Fixed

- **Pause map regressions.** A stale HDMA state is now reset when the pause map
  is opened, map layer offsets use the native layout, and missing map assets
  are repaired on the fly instead of rendering wrong.
- **Autosave recovery.** Autosave data written by older versions is quarantined
  rather than misread, and recovery no longer depends on write ordering that
  the SD card does not guarantee.
- Menu fixes in the figurine viewer and the pause menu.
- Further native-port gameplay fidelity work, including collision handling.

### Known limitations

- True widescreen is not part of this release.
- The updater replaces the game NRO only. Your ROM, saves, assets and settings
  are never touched by it.

## v0.1.2 — sword hitbox hotfix

A minimal collision hotfix. No other gameplay, content or configuration
changes: everything from v0.1.1 is preserved.

### Fixed

- **Sword collision could be mirrored to the wrong side during certain
  right-facing attacks.** The horizontal mirror applied to the sword's
  damage hitbox was not taking effect, so those attacks tested collision on
  the opposite side of Link from the one being attacked.
- Improved native-port gameplay fidelity.

### Notes

- No save or configuration migration is required.

## v0.1.1 — clean-install hotfix

A first-run and stability hotfix. No gameplay content changes: everything
from v0.1.0 is preserved.

### Fixed

- **Clean-install first boot on real Nintendo Switch hardware.** A fresh
  `/switch/tmc/` containing only the NRO and your own ROM now generates its
  runtime assets and reaches the title screen.
- **Build-state path handling on `sdmc:/`.** The Switch device prefix broke
  path canonicalisation, so a successful extraction was recorded as a
  failure.
- **Repeated asset extraction.** The cache state marker was never written,
  so every launch re-extracted from scratch.
- **Extraction stability and filesystem durability**, including bounds and
  overflow hardening in the asset reader and LZ77 self-reference handling.
- Standard error is routed to an internal sink on Switch; no `/dev/null` is
  used, which previously could be fatal.
- The applet message queue is pumped during extraction, so the system no
  longer sees the app as unresponsive while it works.

### Changed

- **First-run extraction is faster**: development-only editable output is no
  longer written. Expect around a minute or more on the first launch,
  depending on SD-card speed; later launches reuse the cache.
- The contextual Ezlo/hat shortcut now defaults to **Left Stick click
  (LSTICK)** instead of **L**, so L keeps its normal GBA and menu duties.
  `CONTEXT L` is still selectable in CONFIG. An existing configuration still
  on the old default is migrated automatically; a binding you chose yourself
  is left alone.

### Not in this release

- Automatic CPU clock management. It is still being evaluated and no clock
  is touched, exactly as in v0.1.0.
- The update checker, deferred to v0.2.0. Nothing is downloaded or installed
  by this release.

## v0.1.0 — first public release

The first public build of Alek's Ultimate NX Edition. Everything below is
relative to the upstream base this Switch tree was forked from (see
[CREDITS.md](CREDITS.md)).

### Nintendo Switch port

- Native Switch target (libnx + SDL), packaged as homebrew NRO with custom
  icon.
- Threaded software PPU rendering (per-scanline worker pool) for real-hardware
  performance.
- Application-mode oriented; docked and handheld both supported.

### Display layouts

- Three runtime-switchable display modes: **NORMAL**, **DUAL** (game +
  second-screen panel) and **FLIP 270** (vertical composition for physically
  rotated play).
- Configurable geometry: DUAL game scale 2X/3X, DUAL panel size 70–90%, FLIP
  panel size 70–100%, and adjustable screen gaps; NORMAL integer screen
  scaling.
- Quick display-mode switch chord (PLUS + ZR).

### Second screen

- Four-tab companion panel rendered from the game's own ROM-decoded art:
  **QUEST** (Story Guide: quest, objective, hint, location), **MAP**
  (overworld and dungeon maps, live player marker, follow cam, windcrest
  pins, floor auto-return), **ITEMS** (A/B rings plus X/Y/ZL/ZR shortcut
  slots), **CONFIG** (all settings).
- Touchscreen interaction across tabs, settings and item assignment.
- Panel backdrop styles (parchment pattern / cream / dark).
- Contextual R-button **Action Hint** with OFF / CONTEXTUAL / ON setting.

### Controls

- Full rebinding of the six GBA controls plus a Controls Reference screen.
- Dedicated **Talk to Ezlo** shortcut (contextual L by default, rebindable).
- Four extra item shortcut slots (X/Y/ZL/ZR) with persistent assignments.

### RetroAchievements

- Login/logout, game recognition, Rich Presence, real unlock notifications
  with real badge artwork (cached), six toast styles, and a local test
  notification.

### Localization

- Port-owned UI localized into English, Español, Français, Deutsch, Italiano —
  including the Story Guide. Correct accented rendering throughout the panel
  UI. (Nintendo's original in-game dialogue localization is untouched.)

### Saves / system

- Port-managed **Autosave** with **Load Autosave** (separate `autosave.bin`;
  never overwrites the game's own save).
- **Return to Title** with confirmation.
- Read-only **Version** row (v0.1.0).

### Performance / startup

- Substantially reduced startup time by removing a legacy per-page ROM
  extraction step from boot (roughly 13–14 s to title observed on tested
  hardware).
- Second-screen repaint caching so the panel only repaints when its content
  changes.

### Rendering fixes

- Fixed the **X and Y item shortcuts being swapped**: the default bindings had
  the X slot firing on the physical Y button and vice versa. Physical X now
  drives the X shortcut and physical Y the Y shortcut. A config written by an
  earlier build is corrected automatically on first launch, unless its
  shortcut bindings were hand-edited (those are left alone).
- Fixed the Deepwood Shrine rotating-barrel room rendering flat/untextured:
  the affine background's per-scanline HBlank-DMA register updates are now
  applied and the BG2 reference point is modelled as the hardware latch it is,
  fixing both the missing texture and its vertical phase during rotation.
  Verified on real hardware.

### UI polish

- Heart display laid out for the full 20-heart maximum at a constant heart
  size (wraps rows instead of shrinking).
- Larger, more readable rupee counter, consistent across tabs.
