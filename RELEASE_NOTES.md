# v0.7.3-alpha

**"Write a new dynasty save" now works.** It never has in a released build.
Turning it on ended the run with a Node error and no save, whatever roster you
were generating.

## What was wrong

Your roster was generated, correctly, to `Output\Generated_Roster.csv` beside
the executable. The step that writes it into your save then looked for it in
`tools\native-save\Output\Generated_Roster.csv` — a folder that has never
existed — and stopped:

```
Error: ENOENT: no such file or directory, open
  '...\CFB27-Roster-Generator-0.7.2-alpha-win-x64\tools\native-save\Output\Generated_Roster.csv'
```

The save reader is a separate program, and it runs from its own folder because
that is where its code lives. The roster's path is relative to *yours*. Handing
one straight to the other meant the two disagreed about what the same path
meant.

Nothing to do with the size of the roster. A full FBS run just gets that far.

## What now happens

Every path is resolved before the save reader sees it, so both halves mean the
same file. Generate with **Write a new dynasty save** ticked and you get
`YOUR-SAVE-Recreated` beside your original, as the option has always described.

Your own save is still never modified. That has not changed and is checked
rather than assumed.

## If it fails, it now says why

The save reader used to answer with a raw stack trace. It no longer does:

- A table it cannot find is **named**, followed by *"nothing was written and
  your save was not touched."*
- A save that is not there says **there is no dynasty save at that path**,
  instead of complaining about a missing file header.

## Do I need to re-run anything?

**Only if you wanted a save written and did not get one.** If you have been
importing `Generated_Roster.csv` with a roster editor, that file was always
correct and nothing about it changes here.

## Nothing else changed

Ratings, archetypes, abilities, equipment, faces, the commentary index, the
season year and team assignment are identical to v0.7.2.

## Download

**`CFB27-Roster-Generator-0.7.3-alpha-win-x64.zip`** (123 MB)

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
