# v0.9.2-alpha

**Schools the game dropped can now be recreated.** Of the 119 teams on a 2004
roster, exactly one is missing from CFB27 — Idaho — and until now that roster
was refused outright. Idaho now generates onto one of the game's generic FCS
teams.

## What this fixes

Import a 2004 roster and try to generate Idaho, and v0.9.0 stopped with:

```
Error: School 'Idaho' is not in the team mapping file.
```

There was nowhere to put them. CFB27 carries 136 FBS schools and Idaho left the
FBS in 2018.

The game does ship five generic FCS teams — East, Midwest, Northwest, Southeast
and West — each with a real 85-man roster sitting unused. **Idaho is now written
onto FCS East**, and nothing is asked of you: load the roster and generate.

```
Historical roster: 2004 Idaho — 56 players
Converted 56 players onto FCS East (0 skipped, 29 slots filled as depth)
```

## Adding another school yourself

One line in `data/TeamMappings.json`. Put the school's name in the `names` list
of whichever FCS team should hold it:

```json
{ "teamId": 255, "names": ["FCS East", "FCSE", "Idaho"], "standInTeam": "FCS East" }
```

There are five FCS teams, so **five departed schools can live in one dynasty**
before they start overwriting each other. That is plenty for the 2000s, and a
real limit if you go back to the era of Pacific and Long Beach State.

## A bug this found

The equipment step worked out who to re-helmet by asking which players belonged
to the team. That question has a bad answer for the FCS teams: all five share a
team number with the game's entire recruiting pool, so **generating one FCS team
put that era's helmets on 4,527 players who had nothing to do with it.**

Generation now records exactly which roster slots it wrote and equipment follows
those. Ordinary teams behave exactly as before — same slots, same helmets — so
nothing you have generated changes.

## Do I need to re-run anything?

**Only if you generated onto an FCS team**, which before this release was not
really possible on purpose. If you did, re-generate: the equipment table you
imported carried thousands of players it should not have.

Everything else is untouched. Every roster file you have produces exactly what
it did in v0.9.0.

## Nothing else changed

Ratings, imports, depth charts, body builds, archetypes, abilities, faces, the
commentary index and the season year are identical to v0.9.0.

## Download

**`CFB27-Roster-Generator-0.9.2-alpha-win-x64.zip`**

Unzip the whole folder — keep `data`, `templates` and `tools` together beside
the executables — and run `RosterGenerator.Gui.exe`. Windows 10 or 11, 64-bit.

## Known limitations

- **Not code-signed.** Windows SmartScreen will show *"Windows protected
  your PC"*. Click **More info** → **Run anyway**.
- **Five departed schools per dynasty**, because there are five FCS teams.
- **The FCS teams do not appear in the team list.** The game marks them as
  having no team number of their own, so they are reachable by name but not
  listed. Type the school and it works.
- **Imported rosters carry no stats, awards, combine or draft data**, and no
  hometown or previous school. The old files never held them.
- **Imported ratings are estimates anchored to the old roster's ordering**, not
  the old roster's ratings.
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
