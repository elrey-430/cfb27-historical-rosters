# Examples

Ready-made roster files. Copy one, point the generator at it, and see what
comes out before you spend an evening typing your own.

## `2023_Florida_State.csv`

The 2023 Florida State team — 13-1, ACC champions, ten players taken in the
2024 NFL Draft.

75 players. It is the file used to develop and test the generator, so it
shows the format working at full detail: draft positions with overall pick
numbers, All-ACC and All-America honours, awards a player was in contention
for, season statistics, hometowns, transfer origins and depth-chart roles.

Try it:

```
RosterGenerator.Cli.exe validate --roster examples\2023_Florida_State.csv --dynasty C:\path\to\export
RosterGenerator.Cli.exe generate --roster examples\2023_Florida_State.csv --dynasty C:\path\to\export
```

It will generate 75 players onto Florida State and fill the remaining 10
roster slots as depth. Then open `Output\Generation_Report.txt` and read what
it decided — that is the fastest way to understand what the tool is doing.

### Where the data came from

Public sources only: seminoles.com, the ACC's own All-ACC announcements,
NFL draft results, and contemporary reporting. Statistics are season totals
for 2023. Roughly 25 jersey numbers are marked "verify" in the `Notes`
column — they could not be confirmed from a primary source and may be wrong.

### What "full detail" costs you

Nothing, if you do not want it. The same team with only names, positions,
numbers, classes and roles would generate a perfectly usable roster — the
ratings would simply be less precise. Compare the two if you are curious:
delete every column after `Season` and run it again.
