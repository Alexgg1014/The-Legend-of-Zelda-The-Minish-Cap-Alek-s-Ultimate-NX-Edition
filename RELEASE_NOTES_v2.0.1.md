# Alek's Ultimate NX Edition v2.0.1

A hotfix for the pause menu in widescreen, plus five optional tweaks from the
Minish Cap Definitive Edition romhack.

Saves and settings carry over; the in-app updater installs it as usual (copy
`tmc_aleks_ultimate_nx.nro` to `/switch/tmc/` as is if installing by hand).

## Fixed

- **The pause menu looked shifted, with a purple bar stretching from the R
  tab** in widescreen. 2.0.0 kept area-change fades wide, and opening the
  pause menu looked like one of those fades to it, so the 240-column menu was
  shown in a wide frame with its right edge smeared. Menus now drop to the
  native canvas again, as in 1.5.

## New: Definitive Edition options (issue #22)

Five toggles under **SETTINGS → GAMEPLAY → QUALITY OF LIFE**, all **off** by
default, so nothing changes unless you turn them on. Thanks @MoTem for the
suggestion.

- **Fairy charms last 5 min**: Din's, Nayru's and Farore's charms from the
  Great Fairies last 5 minutes instead of 1.
- **Fairies heal 12 hearts**: caught, bottled and the one that revives you
  (normally 4).
- **Shell drops ×5**: each Mysterious Shell from enemies and grass is worth 5.
- **Shop sells 300 shells**: the 30-shell bag in Stockwell's shop holds 300,
  same price.
- **Instant figurine capsule**: Carlov's machine skips the capsule's drop and
  crack animation.

These are reimplemented in the port; nothing is taken from the romhack.

## Includes

Everything from 2.0.0 (randomizer beta, Dual widescreen, volume, cheats and
more) — see the changelog.
