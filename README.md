# CFB27 Historical Roster Generator

Recreate real historical college football rosters inside your CFB27 dynasty.

You type in the players you can find. It writes you back a **dynasty save you
load in the game** — ratings, archetypes, heights, weights, hometowns,
period-correct helmets and the rest of the 85-man squad filled in for you.

```
your dynasty save → [this tool] + your roster → a new dynasty save
```

No export step. No separate roster importer. Nothing to install.

> **Alpha.** It works, it is heavily tested, and people are using it — but see
> [Before you start](#before-you-start), particularly the part about backing up
> your save.

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

You get back a save in which those players exist on that team, with:

- **Ratings from EA's own overall formulas.** Not invented — the actual
  formulas, all 79 of them, one per position/archetype pair. Verified against a
  full dynasty export at **99.33% exact** over 16,257 players.
- **An archetype the game actually uses**, with the overall *and* the
  attributes recomputed to match it. Each archetype's attribute profile is
  measured from the 16,256 players in a real dynasty export rather than written
  down by hand, and every position's default is the archetype the game itself
  picks most often there.
- **Attributes that follow the stats you typed in.** Receiving yards lift
  catching and route running, rushing yards lift ball carrying, sacks lift
  pass-rush moves — every position, not only the skill spots. A statistic you
  could not find never costs a player anything.
- **The announcers saying the right name.** The game picks commentary audio
  from a per-player index; left alone, your Jordan Travis is called by the name
  of whoever held that roster slot, for the whole dynasty. It now follows the
  surname you typed, from a mapping of 5,918 names measured across 146,295
  player rows in nine game-generated saves.
- **Real heights, weights, hometowns, class years and jersey numbers** where
  you supplied them, believable ones where you did not.
- **A full 85-man roster.** Slots you did not fill are given end-of-roster
  depth, so leftover fictional players stop starting ahead of your roster.
- **Period-correct equipment.** Pick 1985 and the team wears Riddell TKs,
  vintage masks, long sleeves and X-large pads. On an all-time roster, each
  player wears *their own* year.
- **A face that is not a real person's.** Most slots in a base save carry the
  head scan of a present-day player, so a recreated 1985 team used to wear them
  under other people's names. It no longer does.
- **The right year on screen**, if you ask for it — see below.

Every value it decided for you is listed in a plain-English report.

## Play the season you recreated

Build a 1985 roster and the game can say **1985**, instead of whatever year
your save happens to start in. Tick *"Play it in 1985"* in the app, or pass
`--dynasty-year roster` on the command line.

It is opt-in, because recreating an old roster inside a present-day dynasty is
a perfectly reasonable thing to want. And when you do ask, the year is *all*
that changes: two fields in your save plus the current-season row each team
keeps — 141 bytes of a 30 MB database. The record book keeps its real dates.

## Before you start

**Back up your dynasty save.** It costs one copy-paste. The tool never modifies
the save you give it — the output is always a new file, and writing over your
original is refused outright — but this is alpha software touching a dynasty
you may have put months into.

**Windows will warn you.** The executables are not code-signed, so SmartScreen
shows *"Windows protected your PC"*. Click **More info** → **Run anyway**. If
you would rather not, the source is public at
[cfb27-roster-generator](https://github.com/elrey-430/cfb27-roster-generator)
and every release here is built from it by
[a workflow you can read](.github/workflows/release.yml).

**It is early.** Every release is built on a Windows machine, all **413 tests**
run there, and both executables are launched and exercised there before
packaging — including starting the bundled runtime that reads your save. But
the desktop app has been clicked through by a handful of people on a handful of
machines. Expect rough edges, and please [open an issue](../../issues) when you
hit one. That feedback is what an alpha is for: the attribute work in v0.4.0,
the commentary fix in v0.6.2 and the season year in v0.7.0 all exist because
somebody reported something.

**Nothing to install.** The download carries everything it needs to read a
save, including its own copy of the Node.js runtime (v22 LTS, MIT licensed,
checksum-verified against nodejs.org at build time). You do not install Node.
You do not run a package manager. That copy is private to this application, so
whatever else on your machine wants a different version cannot break it.

## Getting started

### 1. Download and unzip

Get `CFB27-Roster-Generator-<version>-win-x64.zip` from
[the latest release](../../releases/latest) and unzip the **whole folder**.

```
RosterGenerator.Gui.exe    the app — start here
RosterGenerator.Cli.exe    the same thing from a command prompt
data\                      team, position, rating, archetype and era files
templates\                 roster files to copy and fill in
tools\                     what reads and writes your save
README.txt                 quick start
```

Keep `data\`, `templates\` and `tools\` next to the executables.

### 2. Fill in a roster

Copy `templates\HistoricalRosterTemplate_Basics.csv` and open it in Excel,
Google Sheets or Notepad. There are ready-made [examples](examples/) here too.

**Only `FirstName`, `LastName` and `Position` are required.** Old rosters are
badly documented and you are not expected to find a complete record for every
player. Leave out whatever you do not have.

Two things are worth the effort if you can manage them:

| | Why |
|---|---|
| **`Role`** — `Starter` / `Backup` / `Reserve` / `Walk-on` | The cheapest win by far. Without it, players you supply nothing else for all land within a couple of points of each other. One word separates a starter from a third-stringer. Blank is fine and changes nothing. |
| **Stats, draft position, awards** (in the fuller template) | Makes the ratings genuinely accurate rather than merely plausible. Never required. There is an `AwardContender` column too, for what a player was in the running for without winning — often the only evidence left when a season ended early. |

**Heights go in `HeightInches`: write `74`, not `6-2`.** Excel decides `6-2` is
the 2nd of June the moment it opens the file, which destroys the height before
the tool ever sees it. Feet-inches is still read and converted if it survives,
and reported so you can fix it at the source.

You do **not** need to research a team's walk-ons. Every slot you do not supply
is filled in as believable depth.

See [docs/Roster_CSV_Format.md](docs/Roster_CSV_Format.md) for every column.

### 3. Generate

Open **`RosterGenerator.Gui.exe`** and follow the four steps:

1. **Save file…** — your dynasty, from
   `Documents\EA SPORTS College Football 27\saves`. Opening one takes fifteen
   to thirty seconds; it says so while it works.
2. Browse to your roster CSV. It is checked immediately and tells you about
   anything wrong **before** writing anything.
3. Confirm the team and season. If the school was not in the FBS that year — a
   2010 Sacramento State, say — it says so here.
4. Tick **write a new dynasty save**, and **Play it in \<year\>** if you want
   the game to show that season. Click **Generate**.

### 4. Load it

The new save is written beside your original with `-Recreated` added to the
name. Copy it into your saves folder and load it in the game. Your own save is
untouched.

---

## Working from exported CSVs instead

The original workflow still works, is still first-class, and is what happens if
you would rather this program never opened your save.

Export your dynasty to CSVs with the community export tool, point `--dynasty`
at that folder (or a `.zip` of it), and import the results with the community
roster editor. You get two files in `Output\`:

- **`Generated_Roster.csv`** — the players. Import this.
- **`Generated_Equipment.csv`** — what they are wearing. **Import this too** —
  equipment lives in a different table of the save, so the editor needs both.
  Importing only the first leaves a 1985 team in modern helmets.

`Generation_Report.txt` is written either way, and is worth reading.

> If the editor says **"CSV file is missing required column `_tableIndex`"**,
> you handed it your *input* file. Import the files from `Output\` instead.

`--package MyDynasty.zip` hands the whole dynasty back as one archive with the
generated tables inside it and every other file copied through byte for byte.

## A whole season at once

```
RosterGenerator.Cli.exe template --dynasty DYNASTY-BASE1 --season 2010 --output 2010.csv
```

That writes a blank roster file for **every team that played that year** — for
2010, 119 teams and 10,115 rows — with `Team`, `Season` and `Position` already
filled in. Fill it in (a spreadsheet assistant is very good at this) and
generate it in one run. One roster file can carry any number of teams.

**Teams that were not in the FBS yet are left out and named.** CFB27 ships
today's 138 teams, so a 2010 season built from that list would quietly include
Sacramento State, James Madison, Liberty and a dozen more schools still in the
FCS. The dates live in `data\FbsMembership.json` and are yours to correct — it
is advice, never a refusal.

## Command line

Same engine, same results.

```
RosterGenerator.Cli.exe validate  --roster MyRoster.csv --dynasty DYNASTY-BASE1
RosterGenerator.Cli.exe generate  --roster MyRoster.csv --dynasty DYNASTY-BASE1 --save-out DYNASTY-NEW
RosterGenerator.Cli.exe generate  --roster 1985.csv --dynasty DYNASTY-BASE1 --save-out DYNASTY-1985 --dynasty-year roster
RosterGenerator.Cli.exe list-teams --dynasty DYNASTY-BASE1
```

`--dynasty` takes your save file, a folder of exported CSVs, a `.zip` of one,
or the Player CSV on its own. `validate` checks your file and writes nothing.
Run it with no arguments for the full option list.

## Mistakes it handles for you

You should not lose an afternoon's typing to one bad cell.

| What you did | What happens |
|---|---|
| Left out height, weight, hometown, class… | Filled in from the player being replaced, and listed in the report |
| Typed `#13`, `13.0`, `212 lbs`, `4.49s` | Read correctly, and the report says what it read |
| Let Excel turn a height into `2-Jun` | Named as a date rather than reported as an implausible height, so you look at the right thing |
| Typed a jersey number of `199` | Reported; that player keeps their number, the other 84 are unaffected |
| Misspelled a class or a role | Reported, with the values that work |
| Used a position it does not know | Only that player is skipped, and it names the file to add the alias to |
| Named a school your dynasty does not carry | That team is reported and the other 130 still convert |
| Saved from Excel | The byte-order mark and trailing empty rows are ignored |
| Left a row short, or added a stray comma | Handled, and the row number is reported |
| Listed more players than the team has slots | The first 85 are used, the rest are named |

Everything it decides is written down. It never changes your data silently.

## Frequently asked

**Will it overwrite my dynasty?**
No. The save you give it is never modified — the output is always a new file,
and writing over the original is refused outright. Only fields that actually
differ are written, and the empty roster slots the game pre-allocates are left
exactly as they were.

**Do I need every player's stats?**
No. Name and position are the only required fields. More detail improves the
ratings; none of it is required, and a stat you cannot find never counts
against a player.

**Do I still need the export tool and the roster editor?**
No. Point it at your save and you get a save back. Both community tools still
work if you prefer them, and the CSV route is fully supported.

**Can I do more than one team?**
Yes. One roster file can carry any number of teams — each team's slots are
disjoint, so they all convert into one output. `template --season <year>`
writes the blank file for a whole season.

**My all-time roster is all wearing the same era's helmets.**
Fixed in v0.7.0. Put each player's own year in their own `Season` cell and each
of them gets that year's equipment. Before that the tool read `Season` once per
file and took whichever year was typed first.

**I regenerated an old roster and the attributes changed.**
Expected. Overall ratings stay where they were; what moves is each player's
*shape*, onto the values the game itself gives their archetype. v0.7.0 also
changed which archetype many players get, because the old defaults were
archetypes the game barely uses.

**It says "No player table was found".**
The path you chose is neither a dynasty save nor the folder the export tool
wrote. A save has no file extension — use the **Save file…** button, which
opens in the right folder.

**Opening my save takes twenty seconds. Is it stuck?**
No. Reading a save means unpacking 30 MB of compressed, bit-packed tables, and
it is slower again if your Documents folder is redirected to OneDrive. The tool
says what it is doing while it works.

**What if my team's name does not match?**
Run `list-teams` to see the names your dynasty uses, or add an alias to
`data\TeamMappings.json`. The game spells some schools its own way —
"Mississippi St", "W. Michigan", "Sac State".

**Which seasons get period equipment?**
2010–2016, 2000–2009, 1990–1999, 1980–1989 and anything before 1980. A season
outside those leaves equipment exactly as it was, because only helmets
confirmed to exist in the game are ever written. The ranges live in
`data\EquipmentEras.json`.

**Why is a punter rated so high / a backup so low?**
Open `Generation_Report.txt` and search for the player. Every decision is
explained there, including which signals fired and how confident it was.

**I disagree with one player's ratings.**
Fair enough — it estimates from what you gave it, and on a marquee player you
know better than a formula does. Everything it writes is editable afterwards,
and the report will tell you *why* it landed where it did, which usually points
at a missing award or stat line.

**Can I change how ratings work?**
Yes — everything is in editable JSON in `data\`. Nothing is compiled in.

## Reporting problems

[Open an issue](../../issues). Useful things to include:

- What you expected and what you got
- The relevant lines from `Generation_Report.txt`
- Your roster CSV, if you can share it

Please do **not** attach your dynasty save or a full folder of exported CSVs.
They are large and they are not usually needed.

## How it works, and how it was checked

The interesting details live in the
[source repository](https://github.com/elrey-430/cfb27-roster-generator):
what is known about each column of the save file and how it was confirmed, how
ratings are derived, and the fidelity benchmark.

The short version:

- A generated 2023 Florida State roster tracks the shape of the roster the game
  itself ships for Florida State to within **2.12 overall points per roster
  rank** — closer than a hand-built recreation of the same team managed (3.02).
- Reading a save reproduces the community export tool's own output exactly:
  **4,584,474 field comparisons across 16,257 players, zero mismatches.**
- Unpacking and repacking a save with no edits returns a **byte-identical 30 MB
  database**, on five different dynasties.
- Recreating the 2023 Florida State roster into a real save wrote 5,461 fields,
  changed 85 rows on Florida State and **zero rows anywhere else** in a
  16,500-player table.
- A game patch cannot corrupt your dynasty through this: the save format
  version is checked, and a version this build does not recognise refuses to
  write rather than guessing where fields live.

## Credits and licence

Built on the community's dynasty-export and roster-import tools. This project
started as the step between them and would not exist without them, and both are
still fully supported here.

The generator is MIT licensed — see
[the source repository](https://github.com/elrey-430/cfb27-roster-generator).
It reads and writes the save format using
[madden-franchise](https://github.com/bep713/madden-franchise), also MIT.

Player names, statistics and awards used in the examples are public
information. This is an unofficial fan project with no affiliation with EA
Sports or any school.
