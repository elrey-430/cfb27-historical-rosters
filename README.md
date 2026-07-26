# CFB27 Historical Roster Generator

Recreate real historical college football rosters inside your CFB27 dynasty.

You type in the players you can find. It writes a complete, import-ready
roster — ratings, archetypes, heights, weights, hometowns, and the rest of
the 85-man squad filled in for you.

It works on **CSV files, not save files.** You export your dynasty to CSVs
with the community export tool, this reads those and writes a new CSV, and
the community roster editor imports it back:

```
your dynasty → [export tool] → CSV files → [this tool] → new CSV → [roster editor]
```

> **Alpha.** It works and it is heavily tested, but see
> [Before you start](#before-you-start) — particularly the part about backing
> up your save.

**[Download the latest release →](../../releases/latest)**

---

## What it does

You give it a list like this:

```csv
FirstName,LastName,Position,Number,Class,Role,Team,Season
Jordan,Travis,QB,13,RS Senior,Starter,Florida State,2023
Trey,Benson,Tailback,3,RS Junior,Starter,Florida State,2023
Keon,Coleman,WR,4,Junior,Starter,Florida State,2023
```

It gives you back a player table you import with the community roster
editor, in which those players exist on that team with:

- **Ratings computed from EA's own overall formulas.** Not invented — the
  actual formulas, all 79 of them, one per position/archetype pair. Verified
  against a full dynasty export at 99.33% exact over 16,257 players.
- **A sensible archetype**, with the overall *and the attributes* recomputed
  to match it. A back who caught passes runs routes; a scrambling quarterback
  breaks tackles. Each archetype's attribute profile is measured from the
  16,256 players in a real dynasty export rather than written down by hand.
- **Attributes that follow the stats you typed in.** Receiving yards lift
  catching and route running, rushing yards lift ball carrying, sacks lift
  pass-rush moves — every position, not only the skill spots. A statistic you
  could not find never costs a player anything.
- **Real heights, weights, hometowns, class years and jersey numbers**
  where you supplied them, believable ones where you did not.
- **A full 85-man roster.** Slots you did not fill are given end-of-roster
  depth, so leftover fictional players stop starting ahead of your roster.
- **Period-correct equipment.** Pick 1985 and the team wears Riddell TKs,
  vintage masks, long sleeves and X-large pads. The season you already chose
  is the only input.
- **A face that is not a real person's.** Most slots in a base save carry the
  head scan of a present-day player, so a recreated 1985 team used to wear
  them under other people's names. It no longer does.

Every single value it decided for you is listed in a plain-English report.

## Before you start

**Back up your dynasty save.** This tool writes a file that you then import
with a third-party roster editor. It has been tested carefully, but it is
alpha software touching your save, and the import step is not ours.

**Windows will warn you.** The executables are not code-signed, so
SmartScreen shows *"Windows protected your PC"*. Click **More info** →
**Run anyway**. If you would rather not, the source is public at
[cfb27-roster-generator](https://github.com/elrey-430/cfb27-roster-generator)
and you can build it yourself — and every release here is built from it by
[a workflow you can read](.github/workflows/release.yml).

**It is early.** The release is built on a Windows machine, all 295 tests
run there, and `RosterGenerator.Cli.exe` is launched and exercised there
before packaging. The desktop app has been downloaded and clicked through on
Windows to confirm it opens and works — but only once, by one person, on one
machine. Expect rough edges, and please [open an issue](../../issues) when
you hit one. That is exactly the feedback an alpha is for — the ratings work
in v0.4.0 exists because two people opened one.

**You need two other community tools.** The export tool turns your dynasty
into a folder of CSV files, and the roster editor imports a CSV back into the
game. This project does neither — it is the step in between, and it only ever
reads and writes CSVs.

## Getting started

### 1. Download and unzip

Get `CFB27-Roster-Generator-<version>-win-x64.zip` from
[the latest release](../../releases/latest) and unzip the **whole folder**.

```
RosterGenerator.Gui.exe    the app — start here
RosterGenerator.Cli.exe    the same thing from a command prompt
data\                      team, position, rating and archetype files
templates\                 roster files to copy and fill in
README.txt                 quick start
```

Keep `data\` and `templates\` next to the executables. Nothing to install —
no .NET runtime, no Python, no setup.

### 2. Export your dynasty to CSVs

Run the community export tool on your dynasty. It writes a **folder of CSV
files, one per table** — `Player`, `Team`, and dozens more.

That folder is what you give the generator in step 4. You do not need to work
out which file is which: the Player and Team tables are found for you and the
rest are ignored. (The Player CSV on its own also works; you just have to name
the team yourself.)

### 3. Fill in a roster

Copy `templates\HistoricalRosterTemplate_Basics.csv` and open it in Excel,
Google Sheets or Notepad. There are also ready-made
[examples](examples/) in this repository.

**Only `FirstName`, `LastName` and `Position` are required.** Old rosters are
badly documented and you are not expected to find a complete record for every
player. Leave out whatever you do not have.

Two things are worth the effort if you can manage them:

| | Why |
|---|---|
| **`Role`** — `Starter` / `Backup` / `Reserve` / `Walk-on` | The cheapest win by far. Without it, players you supply nothing else for all land within a couple of points of each other. One word separates a starter from a third-stringer. Blank is fine and changes nothing. |
| **Stats, draft position, awards** (in the fuller template) | Makes the ratings genuinely accurate rather than merely plausible. Never required. There is an `AwardContender` column too, for what a player was in the running for without winning — often the only evidence left when a season ended early. |

You do **not** need to research a team's walk-ons. Every slot you do not
supply is filled in as believable depth.

See [docs/Roster_CSV_Format.md](docs/Roster_CSV_Format.md) for every column.

### 4. Generate

Open **`RosterGenerator.Gui.exe`** and follow the four steps:

1. Browse to the folder of exported CSVs from step 2.
2. Browse to your roster CSV — it is checked immediately and tells you about
   anything wrong **before** writing anything.
3. Confirm the team and season.
4. Click **Generate**.

You get up to three files in `Output\`:

- **`Generated_Roster.csv`** — the players. Import this.
- **`Generated_Equipment.csv`** — what they are wearing, when the season falls
  in a known era. **Import this too** — equipment lives in a different table
  of the save, so the editor needs both files.
- **`Generation_Report.txt`** — every value filled in, corrected, or that
  could not be used, player by player. **Worth reading.**

### 5. Import

Import `Generated_Roster.csv` with the community roster editor — **and
`Generated_Equipment.csv` too if one was written.** They are two different
tables in the save; importing only the first leaves a 1985 team in modern
helmets.

> If the editor says **"CSV file is missing required column `_tableIndex`"**,
> you handed it your *input* file. Import the files from `Output\` instead.

---

## Command line

Same engine, same results, for anyone who prefers it.

`--dynasty` is the folder of exported CSVs (or the Player CSV itself).

```
RosterGenerator.Cli.exe validate --roster MyRoster.csv --dynasty C:\path\to\exported-csvs
RosterGenerator.Cli.exe generate --roster MyRoster.csv --dynasty C:\path\to\exported-csvs
RosterGenerator.Cli.exe list-teams --dynasty C:\path\to\exported-csvs
```

`validate` checks your file and writes nothing. Run it with no arguments for
the full option list.

## Mistakes it handles for you

You should not lose an afternoon's typing to one bad cell.

| What you did | What happens |
|---|---|
| Left out height, weight, hometown, class… | Filled in from the player being replaced, and listed in the report |
| Typed `#13`, `13.0`, `212 lbs`, `4.49s` | Read correctly, and the report says what it read |
| Typed a jersey number of `199` | Reported; that player keeps their number, the other 84 are unaffected |
| Misspelled a class or a role | Reported, with the values that work |
| Used a position it does not know | Only that player is skipped, and it names the file to add the alias to |
| Saved from Excel | The byte-order mark and trailing empty rows are ignored |
| Left a row short, or added a stray comma | Handled, and the row number is reported |
| Listed more players than the team has slots | The first 85 are used, the rest are named |

Everything it decides is written down. It never changes your data silently.

## Frequently asked

**Do I need every player's stats?**
No. Name and position are the only required fields. More detail improves the
ratings; none of it is required. Since v0.4.0 stats are worth more than they
used to be — they shape a player's individual attributes, not just how good he
is overall — but a stat you cannot find never counts against him.

**I regenerated an old roster and the attributes all changed.**
Expected in v0.4.0. Overall ratings stay exactly where they were; what moved is
each player's *shape*, onto the values the game itself gives their archetype.
On a full 85-man team that is typically ~150 attributes moving 30 points or
more. If you had hand-edited a generated roster in the community editor, those
edits live in the editor — regenerating starts from your export again.

**Will it overwrite my dynasty?**
No. It never touches your save — it only reads the exported CSVs and writes a
*new* CSV. Getting anything back into the game is a separate step you take
deliberately with the roster editor.

**It says "No Player table found".**
The folder you chose is not the one the export tool wrote. It should hold many
CSV files, one of them the Player table. A save file cannot be read directly;
export it first.

**Can I do more than one team?**
One team per roster file, one run per team. Run it again for the next team.

**What if my team's name does not match?**
Run `list-teams` to see the names your dynasty uses, or add an alias to
`data\TeamMappings.json`. The game spells some schools its own way —
"Mississippi St", "W. Michigan".

**My 1985 team is wearing modern helmets.**
You imported only `Generated_Roster.csv`. Import `Generated_Equipment.csv` as
well — it is a separate table in the save.

**Which seasons get period equipment?**
2010–2016, 2000–2009, 1990–1999, 1980–1989 and anything before 1980. A season
outside those leaves equipment exactly as it was, because only helmets
confirmed to exist in the game are ever written. The ranges live in
`data\EquipmentEras.json` and are editable like everything else.

**Why is a punter rated so high / a backup so low?**
Open `Generation_Report.txt` and search for the player. Every decision is
explained there, including which signals fired and how confident it was.

**I disagree with one player's ratings.**
Fair enough — it estimates from what you gave it, and on a marquee player you
know better than a formula does. Everything it writes is editable in the
community roster editor afterwards. The report will at least tell you *why* it
landed where it did, which usually points at a missing award or stat line.

**My all-time roster is all wearing the same era's helmets.**
The tool takes one season per run, so a file with a different `Season` on every
row resolves to one of them and the equipment era follows that. Nothing else
about an all-time roster is affected.

**Can I change how ratings work?**
Yes — everything is in editable JSON in `data\`. Nothing is compiled in.

## Reporting problems

[Open an issue](../../issues). Useful things to include:

- What you expected and what you got
- The relevant lines from `Generation_Report.txt`
- Your roster CSV, if you can share it

Please do **not** attach your full folder of exported CSVs — it is large and
it is not usually needed.

## How it works, and how it was checked

The interesting details live in the
[source repository](https://github.com/elrey-430/cfb27-roster-generator):
what is known about each column of the save file and how it was confirmed,
how ratings are derived, and the fidelity benchmark.

The short version: a generated 2023 Florida State roster tracks the shape of
the roster the game itself ships for Florida State to within **2.07 overall
points per roster rank** — closer than a hand-built recreation of the same
team managed (3.02).

## Credits and licence

Built on the community's dynasty-export and roster-import tools; this project
does neither and would not be possible without them.

Player names, statistics and awards used in the examples are public
information. This is an unofficial fan project with no affiliation with EA
Sports or any school.

## Licence

The generator is MIT licensed — see
[the source repository](https://github.com/elrey-430/cfb27-roster-generator).

The rosters and examples here are compiled from public information. This is
an unofficial fan project with no affiliation with EA Sports or any school,
and it depends on the community's dynasty-export and roster-import tools,
which are separate projects with their own authors and terms.
