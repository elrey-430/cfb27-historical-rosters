# v0.6.1-alpha

A patch for one problem reported against v0.6.0: **opening a dynasty save
looked like nothing was happening.**

If you tried the new save workflow and the Generate button stayed grey, or the
window went unresponsive, or the command line printed nothing — that was this.
Nothing was broken. The tool was working, silently, for longer than anyone
would reasonably wait.

## What was wrong

Reading a dynasty save unpacks 30 MB of compressed, bit-packed tables and
writes the ones the generator needs back out. That takes about fifteen seconds
on a fast local disk, and noticeably longer if your Documents folder is
redirected to OneDrive — which is where the game keeps saves for most people.

The tool said nothing while it did that.

- The **command line** printed its first line only *after* the unpacking
  finished. Run it, and you got a blank console for half a minute.
- The **desktop app** was worse: it did the work on the same thread that draws
  the window, so the whole app froze. Windows renders that as *"Not
  Responding"*.
- And if the dynasty had genuinely failed to open, choosing your roster
  afterwards **overwrote the error message** with "Ready — N players, nothing
  to fix" — a reassuring sentence above a button that would not work, with no
  trace of what had gone wrong.

## What now happens

- **It says what it is doing, before it does it.** *"Reading DYNASTY-BASE1 — a
  dynasty save takes twenty seconds or so to open."*
- **The window stays responsive** while it reads. The dynasty buttons are
  disabled during the load so a second click cannot race the first.
- **Generate always explains itself.** Whenever it is unavailable, a line
  underneath says which step is missing — or, if the dynasty failed, what
  actually went wrong. That line cannot be overwritten by anything else.
- **The roster check stops saying "Ready".** It reads your roster file and
  nothing else, so it cannot know whether the run is ready. It now says
  **"Roster is fine"**, which is what it actually established.

## Nothing else changed

Same features, same output, same guarantees as v0.6.0 — your save is never
modified, only differing fields are written, empty roster slots are left
alone, and an unrecognised save-format version refuses to write.

If v0.6.0 is working for you, this release only makes it clearer what it is
doing. If it was not working for you, it was probably this.

## Everything in 0.6.0 is still here

- **Drop your dynasty save in, get a dynasty save back.** No export step, no
  separate roster importer. Nothing to install — the download carries its own
  copy of the Node.js runtime.
- **A whole season at once** via `template --season 2010`, with teams that
  were not yet in the FBS left out and named.
- **`HeightInches`** — write `74`, not `6-2`, because a spreadsheet turns
  `6-2` into a date.

See the v0.6.0-alpha notes for the detail on all three.

## Download

**`CFB27-Roster-Generator-0.6.1-alpha-win-x64.zip`** (123 MB)

Unzip the whole folder — keep `data`, `templates` and `tools` together beside
the executables — and run `RosterGenerator.Gui.exe`. Windows 10 or 11, 64-bit.

## Known limitations

- **Not code-signed.** Windows SmartScreen will show *"Windows protected
  your PC"*. Click **More info** → **Run anyway**.
- **Opening a save is still slow**, about fifteen to thirty seconds. It now
  tells you rather than making you guess. Exports open as quickly as they
  always did.
- **Writing a save is verified on one game version.** If a patch moves the
  save format, the tool says so and refuses to write rather than risk your
  dynasty.
- **Windows only.**
- **FBS membership records arrivals, not departures.** A 2010 template writes
  119 teams where the real FBS had 120 — the missing one is Idaho, which
  CFB27 does not carry.
- **The app names the new save for you**, beside your original with
  `-Recreated` appended.
- **Back up your dynasty save** before loading anything new.

---

Source, schema research and the rating model:
[cfb27-roster-generator](https://github.com/elrey-430/cfb27-roster-generator)
