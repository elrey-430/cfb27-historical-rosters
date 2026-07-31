# v0.7.2-alpha

**If you built a roster covering more than one team, only one of them was
generated.** Fixed. Please re-run any multi-team roster you made before this.

## What was wrong

The app sent the team it had detected on every run, and an explicit team used
to beat each row's own `Team` cell. So a file covering a whole season was
written onto whichever school happened to be listed first — **10,115 players
onto one team**, and nothing said so.

It looked like a limitation ("I can only pick one team") and it was actually a
silent wrong answer. A three-team test file put all six players, including
Alabama's and Michigan's, onto Florida State.

## What now happens

**Your `Team` column decides.** Each player goes to the team their own cell
names, so one file carries as many teams as you like and they all generate in
one run:

```csv
FirstName,LastName,Position,Team,Season
Jordan,Travis,QB,Florida State,2023
Jalen,Milroe,QB,Alabama,2023
```

Verified on a filled 2010 season against a real save: **10,115 players across
119 teams, 85 each, zero misplaced** — and that run had a team explicitly set,
which is exactly the case that used to break.

**The app stops asking a question your file already answers.** When your file
names teams, the picker greys out and the window tells you what it found:

> Your file covers 119 teams and each player goes to the one their Team cell
> names — Air Force, Akron, Alabama and 116 more.

The picker stays for the one case it is for: a file with **no** `Team` column,
where it is the only way to say where players go.

On the command line, `--team` is now a fallback for rows that leave `Team`
blank rather than something that overrides the file. If you want one team,
put one team in the file.

`--season` is unchanged and is still a true override — a season really is
roster-wide, so "treat this file as 1999" still means all of it.

## Do I need to re-run anything?

**Yes, if your roster file named more than one team.** What you got was one
team's worth of players and 118 teams quietly dropped. Re-generate and you will
get all of them.

If your file only ever named one team, nothing about your output changes.

## Nothing else changed

Ratings, archetypes, abilities, equipment, faces, the commentary index and the
season year are identical to v0.7.1.

## Download

**`CFB27-Roster-Generator-0.7.2-alpha-win-x64.zip`** (123 MB)

Unzip the whole folder — keep `data`, `templates` and `tools` together beside
the executables — and run `RosterGenerator.Gui.exe`. Windows 10 or 11, 64-bit.

## Known limitations

- **Not code-signed.** Windows SmartScreen will show *"Windows protected
  your PC"*. Click **More info** → **Run anyway**.
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
