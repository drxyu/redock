# redock

**Unplug the monitors. Keep the layout.** Every window back on the right screen, in the right spot, with one command.

Running a Mac with two or three external displays? Then you know the drill. You undock for a meeting, come back, plug everything in, and macOS has dumped your windows onto one screen. Now you get to spend five minutes dragging Slack, your editors, the browser and a dozen terminals back where they belong. Every single time.

Two commands end that:

```sh
save-windows office      # before you unplug
restore-windows office   # after you plug back in
```

Keep a layout for every desk you work at (`home`, `office`, `studio`) and switch between them in one go. Run `restore-windows` on its own to pick a layout from a menu.

- **No app to install.** No menu-bar icon, no background process, no subscription. Two small shell scripts that run on what every Mac already has.
- **The right window, not just any window.** It tracks the window numbers macOS itself assigns, so each of your three Chrome windows goes back to its own spot.
- **No guessing.** A window it cannot identify for certain is reported and left alone, never shuffled somewhere random.

Author: drxyu (https://github.com/drxyu). License: MIT, see [LICENSE](LICENSE).

## Install

```sh
git clone https://github.com/drxyu/redock.git ~/redock
echo 'export PATH="$HOME/redock:$PATH"' >> ~/.zshrc
```

Open a new terminal. The first time you run a command, macOS needs Accessibility permission for your terminal app: System Settings > Privacy & Security > Accessibility. Add the app, switch it on, then quit and reopen it.

Requirements: macOS with the built-in `osascript` and `/bin/bash` (3.2 or newer). Nothing else.

## Usage

```sh
save-windows home        # save every window's place under the tag "home"
save-windows             # list saved tags, newest first, with date and window count
save-windows -l          # same as above
save-windows -l home     # show the windows saved under "home"
save-windows -d office   # delete the tag "office"

restore-windows home     # put the windows back as saved under "home"
restore-windows          # pick a tag from a menu
```

In the menu, use up/down or j/k to move, Enter to restore, and q or Esc to cancel. Other keys are ignored.

Tags may contain letters, digits, dot, dash and underscore, and must start with a letter or digit. Saving to an existing tag replaces it.

## How it works

- `save-windows` records each window's app, title, position, size and macOS window number.
- `restore-windows` finds each window by its window number first, then confirms the move actually landed on that window. Windows that were closed and reopened since the save are matched by exact title only. Anything it cannot match is reported as not placed and left where it is.
- Both scripts print one line per window, so you can see what was saved, moved or skipped.

## Files

- Layouts are stored as `~/.window-layouts/<tag>.json`. Set `WINDOW_LAYOUT_DIR` to use another folder.
- An older single layout at `~/.window-layout.json` is moved to the tag `default` the first time either script runs.

## Troubleshooting

- "Accessibility permission is missing": grant it to the terminal app as described under Install, then quit and reopen that app.
- Windows reported as not placed: the window was closed, or its title changed since the save. Save again while the window is open.
- A window lands somewhere unexpected: the displays may be arranged differently from when you saved. Save a separate tag for each arrangement.

## Disclaimer

These scripts move and resize windows on your screen through macOS Accessibility. They are provided as is, without warranty of any kind. You use them at your own risk. The author is not responsible for lost work, misplaced windows, data loss or any other damage arising from their use. Review the scripts before running them, and save your work first.

See [LICENSE](LICENSE) for the full terms.
