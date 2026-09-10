# Alek's Ultimate NX Edition v1.3.3

**Update even if none of the achievement bugs affect you: this release fixes a
way your save file could be destroyed.**

Four fixes: a save that could be lost by closing the game at the wrong moment, a
crash that blocked the Wake-Up Mushroom errand, achievements that did not
actually work offline despite v1.3.0 saying they did, and badges that stayed
grey after you earned them.

# A save could be lost by closing the game

This is the important one.

The game commits a save eight bytes at a time -- about 160 writes for one file
-- and until now it rewrote the whole save file after every single one of them,
starting each rewrite by emptying it. For a fraction of a second, ~160 times per
save, `tmc.sav` was either empty or half-written.

Close the game in one of those moments -- exiting homebrew from the HOME menu is
enough -- and the file was left too short. On the next boot the game read it,
found a broken header, and did what a game does with a broken save file: it
formatted it. All three files, gone.

Saves are now written to a temporary file and swapped into place, so the real
file is only ever replaced by a complete one, and a save that is not a whole
image is refused instead of being formatted over. A backup of what you started
the session with is kept beside it as `tmc.sav.bak`.

If this already happened to you, look in SETTINGS -> LOAD AUTOSAVE before
starting over: the autosave ring is written safely and was never affected.

# The Lake Hylia crash

## What happened

An NPC asks a table for its sprite parts using its own `type` as the index.
The table has 21 entries. The NPC that crashed arrived with type 64.

Nothing checked. The lookup read 43 slots past the end of the table, into the
buffer the linker happens to place right behind it — the game's font colour
data — and handed the sprite loader eight bytes of that as if they were a
memory address. The very next instruction read through it, and the console
took a Data Abort.

On the original Game Boy Advance the same out-of-range read lands on a word of
ROM. It is still the wrong answer, but a ROM address is always readable, so the
NPC merely draws with the wrong parts and the game carries on. That difference
is the whole bug: hardware forgave it, and the Switch does not.

## What was changed

Both lookups of that shape — the townspeople's table and the children's — now
return "no extra sprite parts" for an index outside the table instead of
reading past it. `LoadExtraSpriteData` refuses a null table as well, so any
lookup missed elsewhere degrades into a plainly-drawn NPC rather than a crash.

A build-time check keeps the bound tied to the table's actual size, so the two
cannot drift apart, and fails the build if either guard is removed.

## What is still open

*Why* that NPC carries type 64 is a separate question and is not answered here.
This release stops it from being fatal. If you see someone in Lake Hylia
drawn oddly — a missing head, wrong colours — that is the same NPC, and a
screenshot would help.

# Achievements offline

v1.3.0 added offline achievements: the game caches each step of a
RetroAchievements session so a later boot with no network can replay it from the
SD card instead of failing. It cached the login. It cached the session start. It
did not cache the achievement set itself.

So an offline boot logged you in from the cache, asked the server for the set,
got nothing, and left the list empty — **NO ACHIEVEMENT SET LOADED**, which is
what it says on the tin and no help at all.

The cause is a name change. That list of cacheable steps was written for the
older RetroAchievements flow, where the set arrived in a request called `patch`.
The rcheevos library we use moved to version 12, which fetches the set through a
new request called `achievementsets` — and `patch` is not sent at all any more.
The cache kept faithfully storing everything except the one thing the list is
built from.

**Connect once after updating.** The set has never been written to your SD card,
so the first boot with internet is what saves it. Offline boots after that show
your achievements, and anything you unlock offline still syncs when you
reconnect, as before.

A build check now derives these request names from the rcheevos source itself,
so a future library update that renames one fails the build instead of quietly
emptying the list again.

# Badges stuck in grey

An achievement you had just earned showed its name in green but kept the grey,
locked version of its picture.

RetroAchievements publishes each badge twice — one in colour, one greyed out for
the locked state — under the same id. The game asked for the right one every
time. It just filed both under the same name in `switch/tmc/ra_badges/`, and
every path checks that folder before the network. Whichever image arrived first
won for good, so any badge the grid had already fetched while the achievement
was locked stayed grey forever after.

Cached badges are now filed by state. Nothing to clean up: the old entries are
simply never read again, though you can delete `switch/tmc/ra_badges/` if you
want the few kilobytes back.

## Your assets are fine

Nothing to delete, nothing to regenerate. The fix is entirely in the binary.

## Thanks

To digdat0 on GBAtemp, who sent the crash logs off his SD card. `area=11
room=0` and a fault address of `0x40605040B0A09082` are what turned this from
a guess into four lines of certainty. And to the report of the empty
achievement list on an offline boot, which was the visible end of a bug that
had been silent since v1.3.0. Unlocks earned offline now also sync on the next
boot with a network, rather than waiting for a reconnect that a restarted game
can never see.

## Verification

    SHA-256  2cd63de916b1e9a76645a9f6369df09b1cd911bc5b8e392a9ceaff7059162150
             tmc_aleks_ultimate_nx_v1.3.3.nro
    Size     14210739 bytes
