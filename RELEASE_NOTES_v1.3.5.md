# Alek's Ultimate NX Edition v1.3.5

**If you are on v1.3.3 or v1.3.4, update. Those builds freeze when the game
creates a save file.**

# The freeze

v1.3.3 fixed a way your save could be destroyed. The fix worked, and it was
also far too slow to use.

The game writes a save eight bytes at a time — around 160 of those for one file,
and several hundred when it sets up a brand new game. v1.3.3 made every one of
those eight-byte writes rewrite the whole save file and swap it into place:
create a temporary file, write 8 KB, close it, delete the real one, rename. Four
separate operations on the SD card, per eight bytes.

The result was a freeze of minutes on a new game. Worse, anyone who gave up and
closed the game was left with only a `tmc.sav.tmp`, because the delete had
already happened and the rename never did.

Writes are now gathered up and committed once per frame instead of once per
block. Same protection — the real file is still only ever replaced by a complete
one — for three filesystem operations per save instead of four thousand.

**If this happened to you, your save is not gone.** A complete `tmc.sav.tmp` is
now recognised and restored on the next launch.

# The screen-transition crash

Also in v1.3.4, which nobody was offered: a Darknut's sword slash could be
updated for one moment after its owner disappeared during a room transition, and
looked up the owner's data before the check that handles exactly that. On a Game
Boy Advance that read lands harmlessly in the BIOS; the Switch faults. Reported
on the Lon Lon Ranch transition — crash, then a white screen, then a freeze.

# The in-game updater

Also from v1.3.4. If "check for updates" said **Update manifest rejected**, that
was real. The file the game downloads to check for updates keeps three changelog
lines, and on a fourth it rejected the whole file rather than ignoring the
extra. v1.3.2 was the first release to ship four, so the updater has been dead
since then.

That part is already fixed for everyone, without updating: the published file
was trimmed back to three lines. v1.3.5 makes it impossible to repeat — the
limit is higher and extra lines are ignored.

# What is still open

Talking to a Tingle sibling in Hyrule Field still crashes, inside a script
command. This release does not fix it; it makes the next crash report say which
command, which is the missing piece.

# Your assets are fine

Nothing to delete, nothing to regenerate.

# Verification

    SHA-256  b672b0a840f0bd790619e74e9cc0a828318ddb719d3911fdc72753d7901b172c
             tmc_aleks_ultimate_nx_v1.3.5.nro
    Size     14218931 bytes
