# v0.8.0-alpha

**Ratings move for everyone in this release.** Re-generate any roster you care
about — the numbers your file produces today are not the numbers it produced in
v0.7.3.

Two things drove it: recreated players were coming out as somebody else's body,
and a generated roster looked nothing like a real one once you lined the two up.

## Recreated players get their own build

Every generated player now gets a body build — **Lean, Thin, Standard,
Muscular or Heavy** — worked out from their position, height and weight. You
supply nothing; the tool already has all three.

Left alone, the build belonged to whoever held the roster slot. That is not a
subtle wrong: generating the 2023 Florida State roster into a real save turned
up a **310 lb guard marked Thin**, a **290 lb defensive tackle marked Thin**,
and a **305 lb defensive tackle marked Standard**.

Ends and tackles come out Muscular, the interior line and defensive tackle
Heavy, kickers and punters Thin, everyone else on the light builds their height
and weight can actually carry. A 6'5" 190 lb receiver is Lean; a 5'10" 190 lb
receiver is not.

## A drafted player is rated like one

**Every drafted player clears 85 overall, and a draft pick now says roughly
what a player was.** The curve runs the whole band:

| Pick | 1 | 5 | 32 | 64 | 100 | 160 | 256 |
|---|---|---|---|---|---|---|---|
| Overall | **99** | 97 | 93 | 90 | 88 | 86 | **85** |

Before this a seventh-round pick came out at **77** — below a player whose row
said nothing about the draft at all.

**A draft slot is a floor and never a ceiling.** Derrick Henry won the Heisman
and went 45th; a season has to be able to outrun where the NFL took someone
months later. It now does:

| | Heisman season | Ordinary season |
|---|---|---|
| Taken 45th | **96** | 92 |
| Taken 240th | **96** | 85 |

Position still binds the top: a receiver taken first overall reaches 99, a
halfback 96, because 96 is the best halfback the game itself carries.

**`UDFA` in the `DraftPick` column caps a player at 85**, where the drafted band
begins, so the two meet rather than overlap. **Leaving the column empty does
neither** — "undrafted" is a statement about the player, an empty column means
you do not know, and most all-time rosters carry no draft data at all.

## Your roster comes out as a curve, not a stack

A generated roster used to arrive in spikes. On the 2023 Florida State file —
64 of whose 75 rows carry no stats, no award and no draft slot — **18 players
landed on exactly 78 and 25 on exactly 68.** The game's own Florida State puts
three to nine players on each value from 69 to 84.

Two fixes, both measured against the game rather than guessed:

- **Depth-chart roles were rated three to five points low.** A starter is now
  78, a backup 73, a reserve 68, a walk-on 64 — each the median the game itself
  carries at those roster ranks. The 75–79 band went from 3 players to 18,
  against the 21 EA carries.
- **Players the file distinguishes in no way are spread** across the range the
  game shows for their role, instead of stacking on one number. It only ever
  moves a player your file says nothing about: one stat, one award or one draft
  slot and they keep their own rating.

| | Biggest stack | Distinct ratings |
|---|---|---|
| Before | 25 players | 20 |
| **Now** | **15 players** | **27** |
| EA's own roster | 9 players | 25 |

The same file always produces the same roster — the ordering never uses chance.

## Recreated players are no longer marked as real people

`IsNIL` marks a slot as holding a real athlete who signed an NIL agreement, and
**the game will not let such a player be edited.** A recreated player inherited
that flag from whoever held the slot, which both claimed something untrue about
them and left them locked.

Every generated player is now written with it off. Since the flag sits on 100%
of the players at 90 overall and above, it was the whole starting eleven of a
recreated roster arriving un-editable.

## Do I need to re-run anything?

**Yes, if you want any of the above.** Nothing about your roster *file* has
changed — the same file simply produces better output. Re-generate and re-load.

If you are happy with a roster you already built, it still works; it just does
not have the builds or the new ratings.

## Download

**`CFB27-Roster-Generator-0.8.0-alpha-win-x64.zip`** (123 MB)

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
