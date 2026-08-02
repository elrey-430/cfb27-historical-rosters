# v0.8.2-alpha

**If your roster file fills in `DraftRound`, re-generate it.** That column was
in the template and the tool never read it, so a player entered as *round 2,
pick 1* was rated as the first pick of the entire draft.

## What was wrong

`DraftPick` was read as an **overall** pick number, always. `DraftRound` was
used only when `DraftPick` was empty.

So writing a second-round selection the way the draft is actually announced —
round 2, pick 1 — produced the number one overall pick, and a player who really
went 33rd came out in the high nineties instead of the low nineties. Anyone
filling in both columns was quietly inflating their best drafted players.

## What now happens

**Write it either way.** Both columns are read together and you get the same
player:

| `DraftRound` | `DraftPick` | Read as |
|---|---|---|
| `2` | `1` | 33rd overall — the first pick of round two |
| *(blank)* | `33` | 33rd overall |
| `2` | `45` | 45th overall — which is the 13th pick of round two |
| `7` | `20` | 212th overall |
| `2` | *(blank)* | the middle of round two |
| *(blank)* | `150` | 150th overall |

The tool tells the two apart by arithmetic rather than by asking you: **a pick
larger than a round holds cannot be a position inside one**, so it is an
overall number. Below that, a round makes the pick a position within it. In
round one the two readings agree, so nothing has to be decided.

**It always tells you how it read your number.** The report says *"Drafted #33
overall (round 2, pick 1)"* — a misunderstanding shows up in the output instead
of hiding inside a rating.

If a round and a pick flatly contradict each other — round 2, pick 200 — the
report says so and the pick is used, being the more specific of the two. A pick
that runs a round long is not flagged: real rounds run past 32 selections when
compensatory picks are awarded, so round 7, pick 240 is ordinary.

## Do I need to re-run anything?

**Yes, if your file fills in `DraftRound` alongside `DraftPick`.** Those players
were rated as though their pick number was an overall one, which made them far
too good — a round-2 pick-1 player was a #1 overall.

**No, if you only ever filled in `DraftPick`** with overall numbers, or left the
draft columns empty. Nothing about those rosters changes.

## Nothing else changed

Ratings, body builds, archetypes, abilities, equipment, faces, the commentary
index and the season year are identical to v0.8.1.

## Download

**`CFB27-Roster-Generator-0.8.2-alpha-win-x64.zip`** (123 MB)

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
