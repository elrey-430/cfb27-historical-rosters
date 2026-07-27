# v0.6.0-alpha

Type in the players you can find; get back a complete, import-ready roster.

**New in this release: it reads and writes your dynasty save directly.** Point
it at your save, hand it a roster, get a new save you load in the game. No
export step. No separate roster importer.

## Download

**`CFB27-Roster-Generator-0.6.0-alpha-win-x64.zip`** (122 MB)

Unzip the whole folder and run `RosterGenerator.Gui.exe`. Nothing to
install — no .NET runtime, no Python, no Node, no setup. Windows 10 or 11,
64-bit.

## New in 0.6.0 — drop your dynasty in, get your dynasty back

Until now this tool sat between two other programs. You exported your dynasty
to CSVs with the community export tool, ran this, then imported the result
with the community roster editor. **Two of those three steps are gone.**

In the app: click **Save file…**, pick your dynasty out of
`Documents\EA SPORTS College Football 27\saves`, choose your roster, and tick
**write a new dynasty save**. From the command line:

```
RosterGenerator.Cli.exe generate --roster 2023_FSU.csv ^
    --dynasty "...\saves\DYNASTY-BASE1" ^
    --save-out "...\saves\DYNASTY-2023FSU"
```

Then load it in the game.

### Nothing to install

The download carries everything needed to read a save, **including its own
copy of the Node.js runtime** (v22.23.1 LTS, MIT licensed, checksum-verified
against nodejs.org while the release is being built). You do not install Node.
You do not run a package manager.

That copy is *private* to this application, so whatever else on your machine
wants a different version cannot break it, and it cannot break them.

It is why the download is 122 MB rather than 66 MB. That is the whole cost,
and it buys:

| Before | Now |
|---|---|
| Export dynasty → run this → import with a roster editor | Run this |
| Three programs | One |
| A 27 MB CSV to place correctly | A save file you load in the game |
| Equipment changes imported as a second file | Written into the same save |

**Nothing is taken away.** The export-to-CSV workflow works exactly as it
always has, is still first-class, and is still what runs if you point
`--dynasty` at an export folder. If the `tools\native-save` folder goes
missing the tool says so plainly instead of failing obscurely.

### What is guaranteed about writing a save

This is your dynasty, so the rules are strict, and they are tested:

- **Your save is never modified.** The output is always a new file, and
  writing over the file you gave it is refused outright.
- **Only fields that actually differ are written.** Every field is read back
  out of the save and compared first. Recreating the 2023 Florida State roster
  wrote 5,461 fields, changed 85 rows on Florida State and **zero rows
  anywhere else** in a 16,500-player table.
- **Empty roster slots are left alone.** A save keeps pre-allocated slots
  holding no player; 243 of them were untouched in that run.
- **A game patch cannot corrupt your dynasty through this.** The save format
  version is checked, and a version this build does not recognise refuses to
  write rather than guessing where fields live.

Before any of this shipped: reading a save was checked against the community
export tool's own output — **4,584,474 field comparisons across 16,257
players, zero mismatches** — and unpacking then repacking a save with no edits
was checked to return a **byte-identical 30 MB database**, on five different
dynasties.

**Back up your save anyway.** It costs you one copy-paste.

## Also new — a whole season at once

```
RosterGenerator.Cli.exe template --dynasty <your dynasty> --season 2010 --output 2010.csv
```

That writes a blank roster file for the **entire year**: every team that
played, each with its 85 slots, `Team`, `Season` and `Position` already filled
in. For 2010 that is 119 teams and 10,115 rows. Fill it in — a spreadsheet
assistant is very good at this — then generate it in one run.

One roster file can now carry **any number of teams**, and they all convert
into a single output.

**Teams that were not in the FBS yet are left out, and named on the way past.**
CFB27 ships today's 138 teams, so a 2010 season built from that list would
quietly include Sacramento State, James Madison, Liberty, Coastal Carolina and
a dozen more schools that were still in the FCS — and nothing in the game
would tell you. The dates live in `data\FbsMembership.json` and are yours to
correct; it is advice, never a refusal.

## Also new — the height column is now `HeightInches`

**Write `74`, not `6-2`.**

If you have been filling the template with Excel or a spreadsheet assistant
and heights kept coming out wrong, this was why: Excel decides `6-2` is the
2nd of June the moment it opens the file, and writes back `2-Jun` or the
number behind that date. The height was destroyed before this tool ever saw
it.

The column name is now the instruction. Feet-inches is still read and
converted — and reported, so you can fix it at the source before a spreadsheet
eats it. Files you already filled in under the old `Height` name keep working
for good.

## Everything from 0.5.0 is still here

- **`SkinTone`**, said by you and never guessed, with face swaps keeping the
  tone the roster slot already had.
- **`--package`**, handing your whole export back as one archive.
- **Attributes measured from the game itself**, for all 59 archetypes.
- **Period-correct equipment**, faces that are not real people's, a complete
  85-man roster, and ratings from EA's own overall formulas.

## Known limitations

- **Not code-signed.** Windows SmartScreen will show *"Windows protected
  your PC"*. Click **More info** → **Run anyway**.
- **Writing a save is verified on one game version.** If a patch moves the
  save format, the tool will say so and refuse to write rather than risk your
  dynasty. Reading an export is unaffected.
- **Windows only.**
- **FBS membership records arrivals, not departures.** A 2010 template writes
  119 teams where the real FBS had 120 — the missing one is Idaho, which
  CFB27 does not carry, so it cannot be recreated.
- **The app names the new save for you**, beside your original with
  `-Recreated` appended. There is no "save as" dialog yet.
- **Lightly used so far.** All 353 tests run on a Windows machine at release
  time, the executable is launched there, and the bundled runtime is started
  there before packaging. Expect rough edges, and please
  [open an issue](../../issues) when you hit one.
- **The generator will not always agree with you about a player.** Everything
  it writes is editable afterwards.
- **Back up your dynasty save** before importing or loading anything.

## Upgrading

Unzip over a fresh folder and use it. Nothing you have already written needs
to change, including roster files using the old `Height` column.

Keep the whole unzipped folder together — `data`, `templates` and `tools` all
need to sit beside the executables. `tools` is new in this release and is what
reads your save.

Ratings are unchanged from 0.5.0.

## Requires

Nothing, if you use the save workflow. The community export tool and roster
editor are still supported and still work exactly as before if you prefer
them, or if you would rather this program never opened your save.

---

Source, schema research and the rating model:
[cfb27-roster-generator](https://github.com/elrey-430/cfb27-roster-generator)
