# Alek's Ultimate NX Edition v1.4.2

Hotfix for the two reports on 1.4.1. Saves and settings carry over.

The in-app updater installs it as usual when your game is at
`/switch/tmc/tmc_aleks_ultimate_nx.nro` (the documented install path). If you
kept the downloaded file name (`..._v1.4.x.nro`) the 1.4.1 updater could not
find it — that is one of the two bugs fixed here; copy this NRO over yours once
and from now on the updater works with any name.

## Fixed

- **Crash entering Lake Hylia from Lon Lon Ranch.** The room's entity list in
  the SD `assets/` folder (extracted by an early version; that folder is never
  refreshed by updates) has no end marker, so the game read past it and
  spawned garbage. Every room's entity list is now checked when the area
  loads and, if truncated, the ROM's own copy is used instead. No need to
  touch or delete your `assets/` folder. Thanks @JJTapia19 for the crash logs.
- **In-app updater "Current game NRO missing or invalid".** The updater now
  replaces the NRO you actually launched, whatever its file name. Thanks
  @digdat0 for the update log.

## Since 1.4.1

- Crash talking to the girl with the cat after the library quest, Link stuck
  on doors with 360° stick, Ezlo box over the rupee counter, Pegasus dash
  turning with the stick — see the 1.4.1 notes.
