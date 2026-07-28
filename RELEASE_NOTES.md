# v0.6.2-alpha

**Your recreated players were being called by somebody else's name.** Fixed.

The game stores a **commentary index** on every player, choosing which
recorded name the announcers use. This tool never set it, so a recreated
player kept whatever the roster slot already held. Your Jordan Travis was
announced as the player he replaced — every game, all season, for the whole
dynasty. Nothing in the game tells you, which is why it went unnoticed.

## What now happens

**Nothing.** That is the point — there is nothing to fill in, no new column,
and no option to tick. The `LastName` already in your roster CSV sets the
index.

A surname the commentary has no recording of gets **0**: the announcers simply
do not say the name, rather than saying the wrong one. That is the game's own
value, held by a fifth of the players in an untouched save.

On the 2023 Florida State roster that works out as **61 of 85 players named
and 24 left unnamed**, and the generation report gives you those counts.

## Measured, not guessed

The mapping covers **5,918 surnames**, read out of **146,295 player rows across
nine dynasty saves the game generated itself** — where the game assigned both
the name and the index.

Hand-edited rosters were deliberately kept out of that measurement, and the
reason showed up in the data: a roster editor can leave the index pointing at a
slot's previous occupant. One such save disagreed with all nine untouched ones
on exactly the names somebody had typed in — Bush, Palmer, Allen, Leinart, an
All-Time USC roster. Including it would have taught the tool names the
announcers cannot actually say.

**The game agrees with the rule.** Renaming two players in-game and exporting
again shows the game rewriting the index itself, to exactly the values this
mapping gives for the new surnames — including 0 for a surname it has no audio
for.

## What this does not change

Ratings, equipment, faces, hometowns, and every output file are identical to
v0.6.1. Existing roster CSVs work unchanged.

Re-generating a roster you already made will change the commentary index on
those players, and nothing else.

If `data\CommentaryIds.json` goes missing, the field is left exactly as it was
rather than zeroed — "we know nothing" is not the same as "the name cannot be
said" — and the report tells you which happened.

## Everything from 0.6.x is still here

- **Drop your dynasty save in, get a dynasty save back.** No export step, no
  separate roster importer, nothing to install.
- **A whole season at once** via `template --season 2010`, with teams that
  were not yet in the FBS left out and named.
- **`HeightInches`** — write `74`, not `6-2`, because a spreadsheet turns
  `6-2` into a date.
- **It tells you what it is doing** while opening a save, and always says why
  Generate is unavailable.

## Download

**`CFB27-Roster-Generator-0.6.2-alpha-win-x64.zip`** (123 MB)

Unzip the whole folder — keep `data`, `templates` and `tools` together beside
the executables — and run `RosterGenerator.Gui.exe`. Windows 10 or 11, 64-bit.

## Known limitations

- **Not code-signed.** Windows SmartScreen will show *"Windows protected
  your PC"*. Click **More info** → **Run anyway**.
- **A name the commentary never recorded cannot be added.** 0 is the honest
  answer; the audio does not exist in the game.
- **Two surnames are ambiguous** — the game uses two different indexes for
  "Butts" and for "David". The more common one is used.
- **Opening a save takes fifteen to thirty seconds.** It says so while it
  works.
- **Writing a save is verified on one game version.** If a patch moves the
  save format, the tool says so and refuses to write rather than risk your
  dynasty.
- **Windows only.**
- **Back up your dynasty save** before loading anything new.

---

Source, schema research and the rating model:
[cfb27-roster-generator](https://github.com/elrey-430/cfb27-roster-generator)
