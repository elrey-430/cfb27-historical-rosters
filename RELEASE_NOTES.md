# v0.5.0-alpha

Type in the players you can find; get back a complete, import-ready roster.

Works on **CSV files, not save files**: export your dynasty to CSVs with the
community export tool, point this at that folder, and import the CSVs it
writes with the community roster editor. Your save is never opened here.

## Download

**`CFB27-Roster-Generator-0.5.0-alpha-win-x64.zip`** (66 MB)

Unzip the whole folder and run `RosterGenerator.Gui.exe`. Nothing to
install — no .NET runtime, no Python, no setup. Windows 10 or 11, 64-bit.

## New in 0.5.0 — one file in, one file out

**Give it the `.zip`.** If you moved your export between machines, what you
actually have is a zip, not the folder that was inside it. Unpacking it first
was a step that only existed because the tool could not read an archive. It
can now — there is a **`.zip…`** button beside **Browse** — and on the command
line `--dynasty` takes either.

**And it can hand the whole dynasty back as one file.** `--package
MyDynasty-generated.zip` writes your entire export back out with the generated
tables inside it, instead of loose CSVs for you to place.

What matters there is not that it round-trips. It is that **everything the
tool did not generate comes back byte for byte.** An export is 2,273 files and
this tool understands two of them; the other 2,271 have to survive untouched
or a package is worse than the loose CSVs it replaces. Checked against five
fresh dynasty saves:

| | files in | identical out | replaced |
|---|---|---|---|
| default coach, base roster | 2,273 | 2,271 | Player, CharacterVisuals |
| default coach, live roster | 2,273 | 2,271 | Player, CharacterVisuals |
| custom coach, base roster | 2,275 | 2,273 | Player, CharacterVisuals |
| custom coach, live roster | 2,275 | 2,273 | Player, CharacterVisuals |

**Your dynasty is never modified.** Reading a zip expands it to a scratch
folder that is deleted afterwards; writing always creates a *new* archive. If
you dislike the result you still have the original.

The loose `Generated_Roster.csv` and `Generated_Equipment.csv` are still
written exactly as before — `--package` is an extra, not a replacement.

## Also new — your roster can say what its players looked like

The full template has a new optional **`SkinTone`** column: EA's **1
(lightest) to 8 (darkest)**, the same numbering the game uses. Blank is the
normal case.

**It is something you say, never something the tool guesses.** This generator
will not infer what a real person looked like from their name, their hometown
or the position they played. You know who these players are; it does not. Fill
in the handful of players you care about and leave the rest blank.

### Blank got better too

Replacing a real player's head scan with a generated face used to change how a
player looked as a side effect — which is a strange thing to happen when the
whole point of the swap was to stop them wearing somebody else's likeness.

**A face swap now keeps the skin tone the roster slot already had.** On a
2014 Florida State roster: 6 of 6 requested tones came out exactly right, and
of the other 79 players, **79 kept their tone and none moved.**

### Small print worth knowing

- A value outside 1–8 is **ignored and reported**, not rounded into range.
  Quietly turning a typed `10` into the darkest tone in the game would hand
  you a player you never asked for.
- Setting a tone means **choosing a different face**, not moving a slider. A
  generated head carries its own tone, and each head is only ever used at one.
  If your export happens to have no face at the tone you asked for, the
  nearest is used and the report says so.
- Nothing is invented: every face written already exists in your own export.
- `--faces inherit` ignores the column, with a warning, so the flag cannot
  quietly stop meaning what it says.

## Everything from 0.4.0 is still here

- **Attributes that match the player.** Each archetype's attribute profile is
  measured from the 16,256 players in a real dynasty export, so a back who
  caught passes runs routes and a scrambling quarterback breaks tackles.
- **Stats shape the player, not just the overall.** Receiving yards lift
  catching and route running, sacks lift pass rush, interceptions lift
  coverage — every position. A stat you cannot find never costs anyone
  anything.
- **Faces.** Recreated players do not wear the head scan of a real
  present-day player.
- **Period-correct equipment.** Pick the season and helmets, jersey cut and
  shoulder pads follow it. Remember to import **both** output files.
- **A complete 85-man roster**, ratings from EA's own overall formulas, and a
  plain-English report of every decision.

## Known limitations

- **Not code-signed.** Windows SmartScreen will show *"Windows protected
  your PC"*. Click **More info** → **Run anyway**.
- **The package is a convenience, not yet an import path.** The community
  roster editor imports individual CSVs. Whether it will also take a whole
  folder or archive is not something this project controls — so keep importing
  `Generated_Roster.csv` and `Generated_Equipment.csv` as you do now, and
  treat the `.zip` as one tidy thing to keep or move.
- **Windows only.**
- **Lightly used so far.** All 317 tests run on a Windows machine at release
  time, and the CLI is launched and exercised there before packaging. Expect
  rough edges, and please [open an issue](../../issues) when you hit one.
- **The generator will not always agree with you about a player.** Everything
  it writes is editable in the community roster editor afterwards.
- **Equipment covers head gear, sleeves and pads only.** The uniform loadout
  has 32 slots and three are decoded.
- **One team per run**, and one season per run — so an all-time roster with a
  different year on every row gets one era's helmets.
- **Back up your dynasty save** before importing anything.

## Upgrading

Unzip over a fresh folder and use it. Nothing you have already written needs
to change: `SkinTone` is optional and blank behaves as it always did.

The one thing to expect: **re-generating an existing roster may change some
faces**, because replacements now match the slot's own skin tone rather than
being drawn from the whole pool. Ratings are unchanged from 0.4.0.

From **v0.3.x and earlier**: re-generating changes attributes substantially
(overalls stay put), and importing only the roster file leaves your players in
modern helmets — import the equipment file too.

## Requires

Two other community tools: the export tool that turns your dynasty into a
folder of CSV files, and the roster editor that imports a CSV back into the
game. This project is the step in between and does neither.

---

Source, schema research and the rating model:
[cfb27-roster-generator](https://github.com/elrey-430/cfb27-roster-generator)
