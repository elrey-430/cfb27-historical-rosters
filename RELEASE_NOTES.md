# v0.7.0-alpha

**Play the season you recreated.** Build a 1985 roster and the game says 1985.

Until now a recreated roster was played in whatever year your save happened to
start in — a perfect 1985 Nebraska squad, in 2026. It was the one thing about a
historical recreation you could not fix afterwards in a roster editor, because
the year is not in the roster at all.

```
RosterGenerator.Cli generate --dynasty DYNASTY-BASE1 --roster 1985_Roster.csv ^
    --save-out DYNASTY-1985 --dynasty-year roster
```

In the app: tick **"Play it in 1985"**, which appears once you have chosen a
save to write and a season. Give `--dynasty-year` a year, or `roster` to use
the one your roster file already names.

## It is opt-in, and it only moves the year

Recreating an old roster inside a present-day dynasty is a perfectly
reasonable thing to want, so nothing happens unless you ask. And when you do
ask, **the year is all that changes**: two fields in your save plus the
current-season row each team keeps — 141 bytes of a 30 MB database. Your
players, ratings, helmets, faces and coach come back byte for byte.

The record book keeps its real dates. Philip Rivers' 2003 passing yards stay
in 2003 whatever year you set, because those are real records rather than
anything to do with your dynasty.

Setting a year needs `--save-out` (or the app's save option). The year lives in
a part of the save the community export tool does not write, so there is
nowhere to put it on the CSV route — and if you ask anyway, the report says so
rather than quietly dropping it.

## Also new — all-time rosters wear their own decades

An all-time roster is one team holding fifty years of players, and the tool
used to read `Season` once per file. It took whichever year you typed first, so
an All-Time USC squad went out in 1980's helmets — Reggie Bush included.

**Write each player's own year in their own `Season` cell** and each of them
gets that year's equipment. On an all-time Florida State roster: Deion Sanders
in a Riddell TK with a vintage mask, Charlie Ward in a VSR-4, Jalen Ramsey in a
Revolution Speed, in one run.

A blank `Season` cell still takes the roster's, so a file that names the year
once at the top works exactly as it did.

## Also new — the archetype rules had a second pass

Two odd calls were reported: a Groza-winning kicker classified as a *power*
kicker off a 53-yard field goal, and a 278 lb Anthony Munoz classified as a
*pass protector* by a weight threshold, costing him run blocking.

Rather than re-argue the rules, this release measures what the game itself
does across a base save's 11,730 players. That found bigger problems than the
two reports:

- **Twelve positions defaulted to an archetype the game barely uses.** The
  default is what a player you supplied little evidence for receives — most of
  a researched historical roster. Centres defaulted to an archetype **0 of 403
  centres in the game have**, guards to one held by 1 of 944, tight ends to one
  held by 8 of 756. Every default is now the archetype the game itself uses
  most often at that position.
- **The offensive-line weight rules were worse than nothing.** The game's pass
  protectors are *heavier* than its other tackles (median 309 lb against 305),
  so the "under 295 lb means pass protector" rule caught 13 of 138 real ones
  while mislabelling 86 others. Removed. A 278 lb tackle in 1979 was a normal
  tackle, and weight cannot tell you anything else about him.
- **The heavy-lineman rules are kept**, because the same measurement backs
  them up — a power blocker really is heavier.
- **The kicker was right, for the wrong reason.** 74% of the game's kickers
  and 18 of its top 20 are power kickers, so an award winner belongs there.
  That is now the default, and the accurate archetype is chosen by what it
  actually means in the game: accuracy without a big leg.

On the 2023 Florida State roster, 20 of 85 players changed archetype, **every
overall stayed identical**, and 1,017 attributes moved into the shape the game
actually uses. Roster strength is unchanged; the players are just built more
like the game builds them.

## Also new — the app warns about teams that did not exist yet

CFB27 ships today's 138 teams, so a 2010 roster for Sacramento State, James
Madison or Liberty builds perfectly and is wrong, with nothing in the game to
say so. The command line has said this since v0.6.0; **the app never did**,
which was backwards, because the app is where you pick the team and the year.

A note now sits under the team and season boxes and follows both as you change
them. It is advice, never a refusal — the dates are in
`data\FbsMembership.json` and are yours to correct.

## Everything from 0.6.x is still here

- **Drop your dynasty save in, get a dynasty save back.** No export step, no
  separate roster importer, nothing to install.
- **The announcers say the right name**, from the surname you already typed.
- **A whole season at once** via `template --season 2010`.
- **`HeightInches`** — write `74`, not `6-2`, because a spreadsheet turns
  `6-2` into a date.

## Download

**`CFB27-Roster-Generator-0.7.0-alpha-win-x64.zip`** (123 MB)

Unzip the whole folder — keep `data`, `templates` and `tools` together beside
the executables — and run `RosterGenerator.Gui.exe`. Windows 10 or 11, 64-bit.

## Upgrading

Nothing to migrate. Existing roster CSVs work unchanged, and the season year is
opt-in, so nothing happens to your dynasty's calendar unless you ask for it.

The one thing to expect: **re-generating a roster will change some players'
archetypes**, and with them the shape of their attributes. Their overalls do
not move.

## Known limitations

- **Not code-signed.** Windows SmartScreen will show *"Windows protected
  your PC"*. Click **More info** → **Run anyway**.
- **The season year is verified on the display, not across a full dynasty.**
  It was confirmed by loading a save and reading the year off the screen.
  Whether anything derives from the calendar over several simulated seasons —
  recruiting classes, eligibility — has not been played through. The fields
  that were checked are relative offsets, which is encouraging but is not the
  same thing. **Back up your save.**
- **`--dynasty-year roster` on an all-time file** takes the first year in the
  file, which is whichever player you happened to type first. Give the year
  explicitly there.
- **Writing a save is verified on one game version.** If a patch moves the
  save format, the tool says so and refuses to write rather than risk your
  dynasty.
- **Windows only.**
- **FBS membership records arrivals, not departures.** A 2010 template writes
  119 teams where the real FBS had 120 — the missing one is Idaho, which
  CFB27 does not carry.
- **Back up your dynasty save** before loading anything new.

---

Source, schema research and the rating model:
[cfb27-roster-generator](https://github.com/elrey-430/cfb27-roster-generator)
