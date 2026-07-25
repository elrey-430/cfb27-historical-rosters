# v0.1.0-alpha

The first public build of the CFB27 Historical Roster Generator.

Type in the players you can find; get back a complete, import-ready roster.

## Download

**`CFB27-Roster-Generator-0.1.0-alpha-win-x64.zip`** (66 MB)

Unzip the whole folder and run `RosterGenerator.Gui.exe`. Nothing to
install — no .NET runtime, no Python, no setup. Windows 10 or 11, 64-bit.

## What's in it

- **`RosterGenerator.Gui.exe`** — the app. Point it at your dynasty export
  and your roster CSV, confirm the team, click Generate.
- **`RosterGenerator.Cli.exe`** — the same engine from a command prompt,
  with a `validate` command that checks your file and writes nothing.
- **`data\`** — team, position, rating and archetype files. All editable
  JSON; nothing is compiled in.
- **`templates\`** — roster files to copy and fill in.

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
- A plain-English report listing every value it decided for you.

A generated 2023 Florida State roster tracks the shape of the roster the
game itself ships for Florida State to within **2.01 overall points per
roster rank** — closer than a hand-built recreation of the same team managed
(3.02).

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
  221 tests run there, and `RosterGenerator.Cli.exe` is launched and
  exercised there before packaging. The desktop app has been downloaded and
  clicked through on Windows to confirm it opens and works — but only once,
  by one person, on one machine. Expect rough edges, and please
  [open an issue](../../issues) when you hit one. That is what an alpha is
  for.
- **Replaced players keep the faces of the players they replaced.** The
  save's portrait fields are not yet understood.
- **One team per run.** Recreating a whole season means running it once per
  team.
- **Injured stars are under-rated.** Draft position is the strongest signal
  in the model, and it records where a player was taken rather than how they
  played. Jordan Travis generates ~7 points light because a November injury
  dropped him to the fifth round. This is the next thing being worked on.
- **Back up your dynasty save** before importing anything.

## Requires

The community save-export tool to get your dynasty out of the game, and the
community roster editor to put it back in. This project is the step in
between and does neither.

---

Source, schema research and the rating model:
[cfb27-roster-generator](https://github.com/elrey-430/cfb27-roster-generator)
