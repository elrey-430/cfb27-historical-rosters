# CFB27 Historical Roster Generator

Recreate real historical college football rosters inside your CFB27 dynasty
save.

You type in the players you can find. It writes a complete, import-ready
roster — ratings, archetypes, heights, weights, hometowns, and the rest of
the 85-man squad filled in for you.

> **Alpha.** This is the first public build. It works and it is heavily
> tested, but see [Before you start](#before-you-start) — particularly the
> part about backing up your save.

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
- **A sensible archetype**, with the overall recomputed to match it.
- **Real heights, weights, hometowns, class years and jersey numbers**
  where you supplied them, believable ones where you did not.
- **A full 85-man roster.** Slots you did not fill are given end-of-roster
  depth, so leftover fictional players stop starting ahead of your roster.

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

**The app itself has not been clicked through by a human yet.** The release
is built on a Windows machine, all 221 tests run there, and
`RosterGenerator.Cli.exe` is launched and exercised there before packaging —
so the command-line tool is known to start and work on Windows. The window
is tested too, but only in an automated harness. Nobody has yet sat in front
of `RosterGenerator.Gui.exe` and used it. If something looks wrong or does
not open, please [open an issue](../../issues) — that is exactly the
feedback an alpha is for.

**You need the community save-export tool** to get your dynasty out of the
game and the roster editor to put it back in. This project does neither; it
is the step in between.

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

### 2. Export your dynasty

Use the community save-export tool. It writes a folder of CSV files, one per
table. Point the generator at that folder; it finds what it needs itself.

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
| **Stats, draft position, awards** (in the fuller template) | Makes the ratings genuinely accurate rather than merely plausible. Never required. |

You do **not** need to research a team's walk-ons. Every slot you do not
supply is filled in as believable depth.

See [docs/Roster_CSV_Format.md](docs/Roster_CSV_Format.md) for every column.

### 4. Generate

Open **`RosterGenerator.Gui.exe`** and follow the four steps:

1. Browse to your dynasty export folder.
2. Browse to your roster CSV — it is checked immediately and tells you about
   anything wrong **before** writing anything.
3. Confirm the team and season.
4. Click **Generate**.

You get two files in `Output\`:

- **`Generated_Roster.csv`** — this is the file you import.
- **`Generation_Report.txt`** — every value filled in, corrected, or that
  could not be used, player by player. **Worth reading.**

### 5. Import

Import `Generated_Roster.csv` with the community roster editor.

> If the editor says **"CSV file is missing required column `_tableIndex`"**,
> you handed it your *input* file. Import `Output\Generated_Roster.csv`
> instead.

---

## Command line

Same engine, same results, for anyone who prefers it.

```
RosterGenerator.Cli.exe validate --roster MyRoster.csv --dynasty C:\path\to\export
RosterGenerator.Cli.exe generate --roster MyRoster.csv --dynasty C:\path\to\export
RosterGenerator.Cli.exe list-teams --dynasty C:\path\to\export
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
ratings; none of it is required.

**Will it overwrite my dynasty?**
No. It reads your export and writes a *new* file. Importing is a separate
step you take deliberately with the roster editor.

**Can I do more than one team?**
One team per roster file, one run per team. Run it again for the next team.

**What if my team's name does not match?**
Run `list-teams` to see the names your dynasty uses, or add an alias to
`data\TeamMappings.json`. The game spells some schools its own way —
"Mississippi St", "W. Michigan".

**Why is a punter rated so high / a backup so low?**
Open `Generation_Report.txt` and search for the player. Every decision is
explained there, including which signals fired and how confident it was.

**Can I change how ratings work?**
Yes — everything is in editable JSON in `data\`. Nothing is compiled in.

## Reporting problems

[Open an issue](../../issues). Useful things to include:

- What you expected and what you got
- The relevant lines from `Generation_Report.txt`
- Your roster CSV, if you can share it

Please do **not** attach your full dynasty export — it is large and it is not
usually needed.

## How it works, and how it was checked

The interesting details live in the
[source repository](https://github.com/elrey-430/cfb27-roster-generator):
what is known about each column of the save file and how it was confirmed,
how ratings are derived, and the fidelity benchmark.

The short version: a generated 2023 Florida State roster tracks the shape of
the roster the game itself ships for Florida State to within **2.01 overall
points per roster rank** — closer than a hand-built recreation of the same
team managed (3.02).

## Credits and licence

Built on the community's save-export and roster-import tools; this project
does neither and would not be possible without them.

Player names, statistics and awards used in the examples are public
information. This is an unofficial fan project with no affiliation with EA
Sports or any school.

## Licence

The generator is MIT licensed — see
[the source repository](https://github.com/elrey-430/cfb27-roster-generator).

The rosters and examples here are compiled from public information. This is
an unofficial fan project with no affiliation with EA Sports or any school,
and it depends on the community's save-export and roster-import tools, which
are separate projects with their own authors and terms.
