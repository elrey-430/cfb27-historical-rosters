# v0.8.3-alpha

**Your starters now actually start.** Depth charts are rebuilt automatically
when a roster is generated. Nothing is asked of you.

## What was wrong

A depth chart points at roster *slots*, in the order the dynasty's original
players ranked. Generating a roster changes who occupies each slot and left the
chart alone — so the spot the game believed was your starting quarterback held
whoever happened to land there. Often a walk-on.

The game never fixes this on its own. You kick off with the wrong eleven.

## What now happens

Every team you generate has its depth chart rebuilt from the ratings the tool
just produced. Generating the 2023 Florida State roster, before and after:

```
before   QB   Daniels(77), Willow(76), Sperry(74)
after    QB   Travis(92), Glenn(77), Rodemaker(70)

before   WR   Robinson(92), Danzy(84), Lopez(79), ...
after    WR   Coleman(93), Wilson(88), Douglas(80), ...
```

**Every slot, not just the obvious ones.** The chart has 35 of them and fifteen
are not positions at all — the gunner spot takes halfbacks and receivers, long
snapper is mostly tight ends, slot corner draws on corners and both safeties.
Which players belong where, and how deep each spot runs, was measured from the
game's own 143 charts rather than guessed.

**Both sides of the line are handled properly.** Left and right tackle are one
assignment rather than two picks: the game never puts the same player at the
top of both, and the better of the two goes to the left side. Your line comes
out the same way:

```
LT   Byers(LT 84), Armella(RT 70)
RT   Scott(LT 73), Sapp(RT 65)
```

**Anything you pinned yourself is left alone.** The game records locked depth
chart entries separately, and those are never rewritten.

## Do I need to re-run anything?

**Yes, if you want your depth charts fixed** — a roster generated before this
still has the old ordering. Nothing about your roster file changes; re-generate
and re-load.

This only reaches dynasties written as a save. A folder from the community
export tool usually does not carry the depth chart tables, and is skipped
without complaint.

## One thing to know

Reading a save now takes about **13 seconds longer**, because two more tables
come out of it. That is the whole cost.

## Nothing else changed

Ratings, body builds, archetypes, abilities, equipment, faces, the commentary
index and the season year are identical to v0.8.2.

## Download

**`CFB27-Roster-Generator-0.8.3-alpha-win-x64.zip`** (123 MB)

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
