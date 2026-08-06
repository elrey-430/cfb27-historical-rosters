# v0.8.4-alpha

**The tool now writes roster files as well as reading them.** Point it at a
dynasty and get back a spreadsheet in the same format the generator reads —
every player, already filled in.

## Why this matters

Until now the tool only went one way. If you wanted to correct one player out
of ten thousand, your choices were to retype the roster or to edit the result
in a third-party save editor — where the correction was invisible to this tool
and lost the next time you generated. And every new project started from a
blank template rather than from the roster the game already had.

Now it goes both ways. Export, fix the row, generate.

## How to use it

**In the app:** load a dynasty and click **Export roster file**. No roster file
of your own is needed — this is how you get one.

**On the command line:**

```
RosterGenerator.Cli export --dynasty <save or folder> --output MyRoster.csv
```

Add `--team "Florida State"` for one team. **Leave it off and it writes every
team the dynasty carries** — a whole season in one file, which the generator
reads straight back, because the roster file's own Team column decides where
each player goes.

## What comes out

Everything the save actually knows, in the template's exact column order:

- name, position, jersey number
- height, weight, class year, redshirt status
- hometown, home state, previous school
- skin tone
- role — read off the dynasty's own depth chart

**Identity survives the round trip exactly.** Florida State exported from a
base save and generated straight back in, compared player by player: position,
jersey, height, weight, class, redshirt, town, state and previous school —
85 of 85 on every one of them.

**Transfers from schools your dynasty does not model** come out as `Unlisted`
rather than blank. Blank would have read back as "never transferred", which is
a different and untrue thing.

## What comes out empty, on purpose

The evidence columns — **stats, awards, combine numbers, draft position** — are
left blank.

A save records what a player *is*, never what he *did*. There is no stat line
in there to export, and writing one anyway would put invented numbers in your
file. So an exported roster reproduces identity exactly and rates from scratch.
Fill those columns in yourself and the ratings become yours.

That also means **exporting a roster and generating it back is not a no-op** on
ratings. The players are the same men with the same builds; what they're rated
comes from whatever evidence you supply.

## Do I need to re-run anything?

**No.** Nothing about generating a roster changed in this release. This adds a
way to get a roster file out; every roster file you already have still works
exactly as it did.

## Nothing else changed

Ratings, depth charts, body builds, archetypes, abilities, equipment, faces,
the commentary index and the season year are identical to v0.8.3.

## Download

**`CFB27-Roster-Generator-0.8.4-alpha-win-x64.zip`**

Unzip the whole folder — keep `data`, `templates` and `tools` together beside
the executables — and run `RosterGenerator.Gui.exe`. Windows 10 or 11, 64-bit.

## Known limitations

- **Not code-signed.** Windows SmartScreen will show *"Windows protected
  your PC"*. Click **More info** → **Run anyway**.
- **Exported rosters carry no stats, awards, combine or draft data**, because
  a save has never held them. See above.
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
