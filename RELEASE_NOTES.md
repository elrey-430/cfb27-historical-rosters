# v0.7.1-alpha

A small update: **your recreated players now get abilities.**

A generated 92-overall edge rusher used to come out with whatever ability tiers
the player he replaced happened to have — often none at all, and sometimes a
fringe walk-on holding somebody else's gold. Now the tiers follow the rating
the player earned.

Nothing to fill in. It comes from the overall the tool already generates, so
the awards, draft position and statistics you typed in reach abilities through
the rating they produced.

## What it can and cannot do

**It sets how good a player is in the ability slots they have. It does not
choose which abilities those are.**

That is not a shortcut — it is what the save allows. Your save stores a
physical ability as a **tier only**: None, Bronze, Silver, Gold or Platinum.
Nothing on the player says *which* ability a slot is. That mapping lives in the
game's own data and depends on position and archetype — slot 4 on a nose tackle
is not slot 4 on a receiver — so no tool editing a save can point a slot at a
different ability.

What *does* change a player's ability set is their **archetype**, which this
tool already chooses from their profile.

On the 2023 Florida State roster:

```
Jared Verse       LE  92   slot 1 Platinum, slot 3 Platinum, slot 5 Gold
Keon Coleman      WR  91   slot 1 Platinum, slot 2 Platinum, slot 3 Platinum, slot 5 Gold
Ryan Fitzgerald   K   90   slot 3 Platinum
Jordan Travis     QB  88   slot 1 Silver, slot 2 Silver, slot 4 Platinum, slot 5 Gold
```

The generation report lists every player who got one.

## Measured against the game, not invented

The distribution is read out of a base save, where the game assigned
everything itself:

| Overall | Players with at least one ability |
|---|---|
| 50–54 | 3.6% |
| 65–69 | 18.3% |
| 80–84 | 74.4% |
| 90–94 | 99.1% |

Tiers follow the same curve — Bronze-heavy at the bottom, 52% Platinum above
95 — and each archetype fills its own slots in the order the game fills them.
Six tests check the tool reproduces those shares to within 4 points, so the
model is held against the game rather than against itself.

## Mental abilities stay as rare as the game makes them

The handful of named abilities — WinningTime, ClearHeaded, FieldGeneral and the
rest — are genuinely elite: **248 of 11,730 players** in a base save carry any,
and of those, 244 carry all three. Essentially nobody below 80 overall.

A player is only ever given one the game has actually been seen giving their
position, so a kicker can get ClutchKicker and a quarterback cannot.

## Also fixed

**A recreated walk-on could inherit the previous player's abilities.** Slots
filled in as end-of-roster depth were left holding whatever the player before
them had — on the Florida State roster, a 63-overall filler was carrying two
Silvers from his predecessor. Every slot on the team is now decided fresh.

## Nothing else changed

Ratings, archetypes, equipment, faces, hometowns, the commentary index and the
season year are all identical to v0.7.0. Existing roster CSVs work unchanged.

Re-generating a roster you already made will set abilities on those players,
and their overalls will not move.

With `--ratings inherit` nothing is written: abilities are read off a generated
overall, and without one there is nothing to read.

## Download

**`CFB27-Roster-Generator-0.7.1-alpha-win-x64.zip`** (123 MB)

Unzip the whole folder — keep `data`, `templates` and `tools` together beside
the executables — and run `RosterGenerator.Gui.exe`. Windows 10 or 11, 64-bit.

## Known limitations

- **Not code-signed.** Windows SmartScreen will show *"Windows protected
  your PC"*. Click **More info** → **Run anyway**.
- **Which ability a slot is cannot be set**, by this or any other save editor.
  Only the tier can. Changing a player's archetype changes their ability set.
- **Eight abilities appear on no player** in a base save — Adrenaline, BellCow,
  Captain, HotHead, Instinct, Rhythm, TeamPlayer, Toughness — so the tool never
  gives them out. They may be unimplemented, or simply rare.
- **The season year is verified on the display, not across a simulated
  dynasty** (unchanged from v0.7.0). Back up your save.
- **Writing a save is verified on one game version.** If a patch moves the
  save format, the tool says so and refuses to write rather than risk your
  dynasty.
- **Windows only.**
- **Back up your dynasty save** before loading anything new.

---

Source, schema research and the rating model:
[cfb27-roster-generator](https://github.com/elrey-430/cfb27-roster-generator)
