# v0.8.1-alpha

**Your roster now comes out looking like a real one.** A small update on top of
v0.8.0, and the one that finally got the shape right — every measure improved
at once.

## What changed

A player your file says little about is held to what the game carries for that
*kind* of player. That limit used to read only their class year, which treated
"young" and "unknown" as the same thing. The game does not.

Measured across 11,730 players on 138 teams — the overall each kind of player
reaches:

| | Freshman | Sophomore | Junior | Senior |
|---|---|---|---|---|
| Starter | 82 | 84 | 87 | 87 |
| Backup | 78 | 77 | 77 | 77 |
| Reserve | 73 | 73 | 73 | 73 |
| Walk-on | 68 | 68 | 68 | 67 |

**Below the starting eleven, class year hardly matters at all.** Backups reach
77 or 78 whether they are freshmen or seniors; reserves reach 73. Only starters
show a real difference by year, and even then it is five points across four
years.

So one limit per class was wrong in both directions at once. It held a
**freshman backup ten points below** where the game puts one, and let a
**senior reserve nine points above**. Your roster now reads role first and
class second.

## What that looks like

| | Biggest stack | Distinct ratings |
|---|---|---|
| v0.7.3 | 25 players | 20 |
| v0.8.0 | 15 players | 27 |
| **v0.8.1** | **8 players** | **32** |
| EA's own roster | 9 players | 25 |

Fifteen freshmen who all came out at exactly 68 in v0.8.0 are now spread across
the range the game actually gives them. The low 80s sit across 80, 81, 82 and
84 instead of stacking on 80.

Ratings also match the shape of EA's own Florida State roster more closely than
in either previous release, so this costs nothing to take.

## Do I need to re-run anything?

**Only if you want the better spread.** Nothing about your roster file changes
— the same file simply produces a better roster. If you generated with v0.8.0
an hour ago, re-generating is worth it; everything else about the output is the
same.

## Still not right

The bottom of a generated roster is heavier than the game's — 28 players under
70 where EA carries 16. That is the walk-on and reserve end, and it is the next
thing to measure.

## Download

**`CFB27-Roster-Generator-0.8.1-alpha-win-x64.zip`** (123 MB)

Unzip the whole folder — keep `data`, `templates` and `tools` together beside
the executables — and run `RosterGenerator.Gui.exe`. Windows 10 or 11, 64-bit.

## Known limitations

- **Not code-signed.** Windows SmartScreen will show *"Windows protected
  your PC"*. Click **More info** → **Run anyway**.
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
