# v0.4.0-alpha

Type in the players you can find; get back a complete, import-ready roster.

Works on **CSV files, not save files**: export your dynasty to CSVs with the
community export tool, point this at that folder, and import the CSVs it
writes with the community roster editor. Your save is never opened here.

## Download

**`CFB27-Roster-Generator-0.4.0-alpha-win-x64.zip`** (66 MB)

Unzip the whole folder and run `RosterGenerator.Gui.exe`. Nothing to
install — no .NET runtime, no Python, no setup. Windows 10 or 11, 64-bit.

## New in 0.4.0 — attributes that match the player

**Two of you reported the same bug from opposite sides, and you were both
right.**

One generated Marcus Allen — a back who caught 34 passes — and got **30 in
every route-running attribute**. Another generated Marqise Lee, a receiver,
and got **34 juke and 30 trucking**. A Jordan Travis who ran for 485 yards
came out at 35 break tackle.

The generator had already worked out what kind of player each of them was: it
put Allen in a receiving back's archetype and Travis in a scrambler's. Then it
built their attributes from a written-down list that named only some of the 56
ratings, and everything the list forgot fell to 30.

### The fix is a measurement, not a better opinion

Writing a better list would just be somebody's opinion of what a receiving
back ought to be good at, and there are 59 archetypes and 56 attributes — 3,304
opinions.

So the generator now measures the game instead. Across the **16,256 players**
in a real dynasty export it works out what the game itself gives every
archetype at every overall — all 59 of them, all 56 attributes — and starts
each recreated player from there.

| | before | now |
|---|---|---|
| Receiving back, route running | 30 / 30 / 30 | what the game gives receiving backs |
| Receiver, juke and trucking | 34 / 30 | what the game gives receivers |
| Scrambling quarterback, break tackle | 35 | what the game gives scramblers |

The check that this is right: feed one of those measured players back through
EA's own overall formula and it returns the overall they were built for, to
within a third of a point, for 56 of the 59 archetypes.

### What a player did now shows up where they did it

Beyond the archetype, the statistics you type in move the attributes they were
earned with. Receiving yards lift catching and route running. Rushing yards
lift ball carrying and elusiveness. Sacks lift pass-rush moves. Interceptions
lift coverage. Every position on the roster, not only the skill positions.

*How far* it moves is measured too — by how much the game's own players of
that archetype differ from each other. Nothing is nudged by a number somebody
picked.

**It only ever moves attributes up.** A 1968 receiver whose catches nobody
wrote down must not be marked down for the gap, so a statistic you cannot find
costs that player nothing.

### A second job now counts

A running back used to be judged only on running, so a back who caught 37
passes tied with a back who caught none. Now a real second role — a back who
caught, a quarterback who ran — adds to the overall, within a limit, so the
roster's shape still comes from the main job.

### What did not change

**Roster strength.** Regenerating a full 85-man team before and after this
release leaves **every single overall identical**. What moved is the shape of
each player, not how good they are: 147 attributes on that roster moved 30
points or more, into the range the game actually uses.

**Your files.** The input format is unchanged. Re-run a roster CSV you already
wrote and you get the same players, better rated.

## Also new in 0.4.0 — recreated players stop wearing real people's faces

A replaced player used to inherit the head of whoever held that roster slot,
and **9,011 of the 16,257 players in a base save wear a scan of a real
present-day player** — around 71 of the 85 slots on a typical team. So most of
a recreated 1985 roster wore recognisable modern faces under other people's
names. On the Florida State example that was 71 slots; it is now 7, and those
7 are leftover slots still carrying their own player.

Those slots now get a **generated face taken from your own export** — never an
invented asset name — with the portrait field written to match. The choice is
seeded from the player's roster slot, so the same roster regenerates
identically every time. Every substitution is listed in the report, and
`--faces inherit` on the command line restores the old behaviour.

**Not attempted: matching a historical player to a real scan.** The scans are
present-day players, so the overlap with any historical season is close to
nil — and guessing what a real person looked like from their name is not
something this tool should do. If you know the right head, name it yourself;
that is your call, not an inference.

## Everything from 0.3.0 is still here

**Period-correct equipment.** Pick the season and the team's head gear, jersey
cut and shoulder pads follow it. You fill in nothing extra; the year you
already chose is the whole input.

| Season | Helmets | Sleeves | Pads |
|---|---|---|---|
| 2010–2016 | Riddell Revolution Speed, Schutt Air XP Pro VTD | Tight | Small |
| 2000–2009 | Riddell Revolution or VSR-4, Schutt Air Advantage | Loose | Medium |
| 1990–1999 | Riddell VSR-4 | Long | Large |
| 1980–1989 | Riddell TK, vintage masks | Long | X-Large |
| before 1980 | Riddell TK, Vintage Two Bar | Long | X-Large |

A season outside those ranges changes nothing at all. Each player's current
helmet decides what they move to, so a squad stays mixed rather than turning
into 85 identical shells, and facemasks follow the position the way the game's
own rosters do it.

### There are two files to import

```
Output\Generated_Roster.csv      the players
Output\Generated_Equipment.csv   what they are wearing
```

**Import both.** Equipment lives in a different table of the save from the
players, so the roster editor needs each file. The equipment file is only
written when something actually changed.

## What it produces

- Ratings computed with **EA's own overall formulas** — all 79 of them, one
  per position/archetype pair. Verified against a full dynasty export at
  99.33% exact across 16,257 players.
- An archetype per player, with the overall recomputed to match it, and now
  the attributes to match it as well.
- Heights, weights, hometowns, class years, jersey numbers and transfer
  origins.
- A **complete 85-man roster** — slots you did not supply are filled as
  end-of-roster depth, so leftover fictional players stop starting ahead of
  your roster.
- Period-correct equipment and a face that is not a real person's.
- A plain-English report listing every value it decided for you and why.

A generated 2023 Florida State roster still tracks the shape of the roster the
game itself ships for Florida State to within **2.07 overall points per roster
rank** — closer than a hand-built recreation of the same team managed (3.02).

## You only need the basics

`FirstName`, `LastName` and `Position` are the only required columns. Old
rosters are badly documented and you are not expected to find a complete
record for every player — everything you leave out is filled in and listed in
the report.

Adding `Role` (`Starter` / `Backup` / `Reserve` / `Walk-on`) is the cheapest
improvement by far: without it, players you supply nothing else for all land
within a couple of points of each other. After that, statistics are now worth
more than they were — they shape the player, not just their overall.

## Known limitations

- **Not code-signed.** Windows SmartScreen will show *"Windows protected
  your PC"*. Click **More info** → **Run anyway**.
- **Lightly used so far.** The release is built on a Windows machine, all
  295 tests run there, and `RosterGenerator.Cli.exe` is launched and
  exercised there before packaging. Expect rough edges, and please
  [open an issue](../../issues) when you hit one. That is what an alpha is
  for.
- **The generator will not always agree with you about a particular player.**
  It rates what you feed it, as fairly as it can, from what the game itself
  does. Where you disagree, everything it writes is editable in the community
  roster editor afterwards.
- **Faces are generated, not chosen.** A recreated player gets a face that is
  nobody in particular rather than a resemblance.
- **Equipment covers head gear, sleeves and pads only.** The uniform loadout
  has 32 slots — gloves, shoes, visors, towels — and three are decoded. The
  rest keep whatever the save had.
- **A few period gaps.** No Revolution two-bar has been confirmed, so 2000s
  quarterbacks take the skill mask; the Riddell TK has no confirmed kicker
  mask, so pre-1990 kickers take the era default.
- **One team per run.** Recreating a whole season means running it once per
  team.
- **Back up your dynasty save** before importing anything.

## Upgrading

Unzip over a fresh folder and use it. No roster file you have already written
needs a change — there is no new column.

What will differ: **re-generating an existing roster changes its attributes**,
and on most players it changes them a lot. Overalls stay where they were.
Faces will change on any slot that carried a real player's scan. If you have
already hand-edited a generated roster in the community editor, your edits are
in the editor, not here — re-generating starts from the export again.

From **v0.2.x**: importing only the roster file leaves your players in modern
helmets; import the equipment file too.

From **v0.1.0-alpha**: `AwardContender` is a new optional column, and rosters
containing players whose draft slot disagrees with their season generate
different ratings for those players; the report names each one.

## Requires

Two other community tools: the export tool that turns your dynasty into a
folder of CSV files, and the roster editor that imports a CSV back into the
game. This project is the step in between and does neither.

---

Source, schema research and the rating model:
[cfb27-roster-generator](https://github.com/elrey-430/cfb27-roster-generator)
