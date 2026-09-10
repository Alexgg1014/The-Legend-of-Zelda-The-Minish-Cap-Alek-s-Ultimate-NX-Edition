# Alek's Ultimate NX Edition v1.3.4

A crash fix, the in-game updater brought back from the dead, and crash reports
that can finally be traced to a function name.

# The updater has been broken since v1.3.2

If "check for updates" has been telling you **Update manifest rejected**, that
was real, and it was not your install.

The manifest is the small file the game downloads to see whether a new version
exists. Its parser keeps at most three changelog lines — and on the fourth it
did not skip the extra, it rejected the *whole file*. v1.3.2 was the first
release whose manifest carried four, so from that moment the updater stopped
working for everybody, and v1.3.3 shipped five and kept it that way.

**This is already fixed for you and does not need this update.** The published
manifest was trimmed back to three lines, so every install on v1.3.1, v1.3.2 or
v1.3.3 can update again right now. That mattered more than shipping a build,
because the copies that most needed a fix were exactly the ones that could not
be told a fix existed.

What v1.3.4 adds is that it cannot happen again: the cap is higher, and a
manifest with too many lines now has the extra ones ignored instead of being
thrown away. Release notes should never be able to break the updater.

# The screen-transition crash

Reported on the Lon Lon Ranch transition: crash on the first try, a white screen
on the second, a freeze on the third.

A Darknut's sword-slash is a separate entity that follows its owner. When a room
is torn down during a transition, one of those can be ticked in the moment after
its owner is gone. The code already handles that — a slash with no owner deletes
itself — but it looked up the owner's data *before* running that check.

On a Game Boy Advance, reading through a null pointer lands in the BIOS: you get
a junk value, and the delete on the next line cleans up anyway. The Switch faults
instead. The check now runs first, which is what it was always meant to do.

# Crash reports that resolve themselves

v1.3.3 started printing an address that lets a crash report be traced back to
the exact function. It printed the wrong one: a crash is saved on the spot and
turned into a readable log by the *next* launch, and the address being printed
belonged to that later launch rather than the one that crashed. It is now
captured at the moment of the crash and travels with the report.

Reports also record which script command was running. A script command that
crashes leaves no trace of itself on the call stack, so a report could say only
that *some* command failed, out of more than a hundred. This names it.

That is what remains open: talking to a Tingle sibling in Hyrule Field still
crashes, inside a script command. This release does not fix it — it makes the
next report say which one.

# Your assets are fine

Nothing to delete, nothing to regenerate.

# Thanks

To digdat0 on GBAtemp, for the crash logs, the Atmosphere reports and the note
about the updater. The Atmosphere reports are what exposed the wrong address in
v1.3.3's own reports, by disagreeing with them.

# Verification

    SHA-256  49e429026c7d6ab6154a8858228f07e3e600d1909f44801981c425b97d55149c
             tmc_aleks_ultimate_nx_v1.3.4.nro
    Size     14218931 bytes
