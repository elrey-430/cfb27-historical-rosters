# v0.9.0-alpha

**Rosters from the PS2-era NCAA Football games can now be read in.** Point the
tool at one and get back a spreadsheet with real players — names, numbers,
heights, weights, class years — for every team on it.

## Why this is the big one

Building a historical roster has always started the same way: typing. Names,
positions, jersey numbers, heights, weights, one player at a time, eighty-five
times per team.

Community "named" rosters for the old NCAA games already contain all of that,
for well over a hundred teams at once, entered by people who cared enough to get
it right. This release reads them.

## How to use it

**In the app:** type the season, click **Import old roster**, pick the file. The
roster file it writes is loaded automatically, so you can go straight to Check
and Generate.

**On the command line:**

```
RosterGenerator.Cli import --legacy <roster file> --season 2004 --output MyRoster.csv
```

Add `--team "Texas"` for one school. Leave it off and it writes **every team in
the file** — a whole season of college football in one spreadsheet.

**The season is required.** These files do not record what year they are, and
the tool will not guess one from the players on it.

## What you get

Everything the old file actually knows, per player:

- first and last name
- position, jersey number
- height, weight, class year
- skin tone
- role, read from the old game's own depth chart

For a 2004 I-A roster that is **7,350 players across 119 teams** in one go.

## What you don't get, and why

**Ratings are not imported.** This is deliberate, and it is worth a paragraph.

The old games stored 18 attributes. This one uses 57. Those 18 make up a little
over half of what EA's own formula uses to work out a player's overall — and
only 42% of it at quarterback. Copying them across would mean inventing the
other half and presenting it as history. On top of that the old numbers are
stored in five or six bits, on a scale that has no fixed relationship to
anything in the modern game.

**What does come across is the order** — and the order is the useful part
anyway, because it is what those roster-makers actually knew.

- Where a player stood on his own team becomes a rating signal, weighted below
  real evidence like a draft pick or an award.
- Where he stood among others at his position shapes his attributes.

That second part is what makes players feel like themselves. Generating the 2004
USC roster, two backs listed one after the other in the source file:

```
HB Reggie Bush    OVR 84   Elusive Back   speed 95  strength 67
HB LenDale White  OVR 83   Power Back     speed 78  strength 88
```

Same team, same standing, completely different players — and nothing but the old
roster's ordering told the tool that.

**Hometown, home state, previous school and redshirt status** are not in these
files at all, and stats, awards and draft position never were. Fill those in for
the players you know about and the ratings become yours.

## Do I need to re-run anything?

**No.** Nothing about generating a roster changed. Every roster file you already
have produces exactly what it did in v0.8.4 — an import counts against nothing,
so a roster you wrote by hand keeps the confidence it always had.

## One thing to know about the results

An old squad is about 62 players against this game's 85, so the bottom of each
roster is still filled in as end-of-roster depth. And the heights and weights
are a roster editor's numbers, not a media guide's — accurate enough to be
useful, secondhand enough to be worth checking on players you care about.

## Also in this release

- The team id map covers all 119 teams, and 118 of them were identified by
  reading the actual roster rather than assuming the ids run alphabetically.
  They don't: USC and Utah sit next to each other.
- Players the old file never named are left out. Whole divisions shipped
  unnamed in those games.

## Download

**`CFB27-Roster-Generator-0.9.0-alpha-win-x64.zip`**

Unzip the whole folder — keep `data`, `templates` and `tools` together beside
the executables — and run `RosterGenerator.Gui.exe`. Windows 10 or 11, 64-bit.

## Known limitations

- **Not code-signed.** Windows SmartScreen will show *"Windows protected
  your PC"*. Click **More info** → **Run anyway**.
- **Imported rosters carry no stats, awards, combine or draft data**, and no
  hometown or previous school. The old files never held them.
- **Imported ratings are estimates anchored to the old roster's ordering**, not
  the old roster's ratings. See above.
- **A team your dynasty does not carry is reported and skipped**, and the rest
  of the file still generates. Check the report if a count looks short.
- **Which ability a slot is cannot be set**, by this or any save editor — only
  the tier. Changing a player's archetype changes their ability set.
- **The season year is verified on the display, not across a simulated
  dynasty.** Back up your save.
- **Writing a save is verified on one game version.** If a patch moves the
  save format, the tool says so and refuses to write rather than risk your
  dynasty.
- **Windows only.**
- **Back up your dynasty save** before loading anything new.

---

Source, schema research and the rating model:
[cfb27-roster-generator](https://github.com/elrey-430/cfb27-roster-generator)
