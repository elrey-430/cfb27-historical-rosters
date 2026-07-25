# v0.3.0-alpha

Type in the players you can find; get back a complete, import-ready roster.

Works on **CSV files, not save files**: export your dynasty to CSVs with the
community export tool, point this at that folder, and import the CSVs it
writes with the community roster editor. Your save is never opened here.

## Download

**`CFB27-Roster-Generator-0.3.0-alpha-win-x64.zip`** (66 MB)

Unzip the whole folder and run `RosterGenerator.Gui.exe`. Nothing to
install — no .NET runtime, no Python, no setup. Windows 10 or 11, 64-bit.

## New in 0.3.0 — period-correct equipment

**A 1985 roster now wears 1985 helmets.** Pick the season you are recreating
and the team's head gear, jersey cut and shoulder pads follow it. You fill in
nothing extra; the year you already chose is the whole input.

| Season | Helmets | Sleeves | Pads |
|---|---|---|---|
| 2010–2016 | Riddell Revolution Speed, Schutt Air XP Pro VTD | Tight | Small |
| 2000–2009 | Riddell Revolution or VSR-4, Schutt Air Advantage | Loose | Medium |
| 1990–1999 | Riddell VSR-4 | Long | Large |
| 1980–1989 | Riddell TK, vintage masks | Long | X-Large |
| before 1980 | Riddell TK, Vintage Two Bar | Long | X-Large |

A season outside those ranges changes nothing at all.

### Your squad stays mixed

Real teams were never uniform, so the generator does not put 85 identical
helmets on a roster. Each player's **current helmet decides what they move
to** — a Riddell wearer takes the Riddell of the day and a Schutt wearer the
Schutt. In the 2000s Riddell's own line splits: a SpeedFlex wearer belongs in
a Revolution, an Axiom wearer in a VSR-4, because both shells were in college
use at once. Players in a brand that did not exist yet — Vicis, Light — take
the era's common shell, since there is no same-brand answer for them.

### Masks follow the position

A kicker gets a kicker cage, a centre a full cage, an edge rusher a robot, a
quarterback an open bar — the way the game's own rosters do it, where 92–98%
of kickers and punters wear a kicker cage. In the 1980s the line is spread
across the heavier vintage masks instead of all wearing one, chosen from each
player's own record so the same roster always regenerates identically.

### There is now a second file to import

```
Output\Generated_Roster.csv      the players
Output\Generated_Equipment.csv   what they are wearing
```

**Import both.** Equipment lives in a different table of the save from the
players, so the roster editor needs each file. The report lists every player
whose helmet changed and what it changed from.

The equipment file is only written when something actually changed, so a
season with no era leaves you with the roster alone, exactly as before.

### How this was worked out

Equipment is not in the player table at all. Exporting one dynasty twice —
identical but for a handful of helmets changed in the roster editor — changed
exactly **one file out of 2,273**, and every asset name the generator writes
was read out of a diff like that rather than guessed. The names are not the
editor's labels: the Riddell VSR-4 is stored as `standardBrady`, and the shell
the editor calls "Schutt Air XP" is the real-world Air Advantage and a
different asset from the Air XP Pro VTD. Nothing unconfirmed is ever written,
which is why an unlisted season leaves your equipment alone instead of
guessing at it.

## The rest of 0.2.x

Everything below arrived in v0.2.0-alpha and is about one thing: **the season
you are recreating outranks what happened to the player afterwards.**

### A draft slot no longer outvotes the season

Draft position is the heaviest signal the generator uses, and the only one
that looks *backwards* from the season being recreated: it records where the
NFL took a player months later, which is a different question from how they
played. An injury, a position the league does not value, or a bad combine
all move it without moving anything that happened on the field.

When a draft slot sits well below what a player's awards and statistics say,
the generator now trusts the record of the season more, and the report
explains why:

```
Draft position counted for less: Drafted #171 overall sits 14 points below
this player's awards (conference player of the year). A draft slot records
where the NFL took someone months later, not how they played in this season.
```

The slot is not thrown away — a late pick is still information — it just
stops overriding the season itself. The rule is deliberately narrow: it fires
on 3 of the 75 players in the Florida State example, and recruiting stars
cannot trigger it, since they predate the season and are no better a witness
to it than the draft is.

### A new `AwardContender` column

For awards a player was **in contention for** without winning — a finalist, a
semifinalist, someone who was in the conversation. Same award names as
`Awards`, scored a few points lower:

```csv
FirstName,LastName,Position,Awards,AwardContender
Jordan,Travis,QB,Conference Player of the Year,Heisman
```

A Heisman finalist therefore out-rates a first-team all-conference winner,
which is the right ordering and is not what taking whichever happens to be a
"win" would produce. It matters most when a season ended early: what a player
was in contention for before an injury is often the truest thing left in the
record.

Blank behaves exactly as if the column were absent — like every other
optional column, it costs nothing to ignore.

### A correction to the Florida State example

Jordan Travis **won** the 2023 ACC Player of the Year and ACC Offensive
Player of the Year and finished fifth in Heisman voting; the example file had
him at first-team all-conference — two tiers low, on the best season on the
team. Corrected. He now generates at 88 rather than 83, led by the award
rather than by the fifth round he went in after breaking his leg in November.

## What it produces

- Ratings computed with **EA's own overall formulas** — all 79 of them, one
  per position/archetype pair. Verified against a full dynasty export at
  99.33% exact across 16,257 players.
- An archetype per player, with the overall recomputed to match it.
- Heights, weights, hometowns, class years, jersey numbers and transfer
  origins.
- A **complete 85-man roster** — slots you did not supply are filled as
  end-of-roster depth, so leftover fictional players stop starting ahead of
  your roster.
- A plain-English report listing every value it decided for you, and now
  every signal it decided to trust less.

A generated 2023 Florida State roster tracks the shape of the roster the game
itself ships for Florida State to within **2.07 overall points per roster
rank** — closer than a hand-built recreation of the same team managed (3.02).
That is a shade wider than v0.1.0's 2.01, and deliberately so: the benchmark
compares against the game's *generic* Florida State roster, and rating a
top-heavy team's best player correctly moves away from that curve.

## You only need the basics

`FirstName`, `LastName` and `Position` are the only required columns. Old
rosters are badly documented and you are not expected to find a complete
record for every player — everything you leave out is filled in and listed
in the report.

Adding `Role` (`Starter` / `Backup` / `Reserve` / `Walk-on`) is the cheapest
improvement by far: without it, players you supply nothing else for all land
within a couple of points of each other.

## Known limitations

- **Not code-signed.** Windows SmartScreen will show *"Windows protected
  your PC"*. Click **More info** → **Run anyway**.
- **Lightly used so far.** The release is built on a Windows machine, all
  272 tests run there, and `RosterGenerator.Cli.exe` is launched and
  exercised there before packaging. The desktop app has been downloaded and
  clicked through on Windows to confirm it opens and works — but only once,
  by one person, on one machine. Expect rough edges, and please
  [open an issue](../../issues) when you hit one. That is what an alpha is
  for.
- **Replaced players keep the faces of the players they replaced.** The
  save's portrait fields are not yet understood.
- **Equipment covers head gear, sleeves and pads only.** The uniform loadout
  has 32 slots — gloves, shoes, visors, sleeves, towels — and three are
  decoded. The rest keep whatever the save had.
- **A few period gaps.** No Revolution two-bar has been confirmed, so 2000s
  quarterbacks take the skill mask; the Riddell TK has no confirmed kicker
  mask, so pre-1990 kickers take the era default.
- **One team per run.** Recreating a whole season means running it once per
  team.
- **Back up your dynasty save** before importing anything.

## Upgrading

Unzip over a fresh folder and use it. Roster files you have already written
still work unchanged — equipment is driven by the season you were already
supplying, so there is no new column to add.

The one thing to know: if your season falls in one of the ranges above you
will now get a **second output file**, and importing only the roster will
leave your players in modern helmets. Ratings are unchanged from v0.2.1.

From **v0.1.0-alpha**: `AwardContender` is also a new optional column, and
rosters containing players whose draft slot disagrees with their season will
generate slightly different ratings for those players; the report names each
one.

## Requires

Two other community tools: the export tool that turns your dynasty into a
folder of CSV files, and the roster editor that imports a CSV back into the
game. This project is the step in between and does neither.

---

Source, schema research and the rating model:
[cfb27-roster-generator](https://github.com/elrey-430/cfb27-roster-generator)
