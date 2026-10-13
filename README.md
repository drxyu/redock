# redock

**Listen up, Mac users with more than one monitor.**

You unplug for a meeting, come back, plug in, and macOS has dumped every window onto one screen. So you drag them back. Window. By. Window. Every. Single. Day.

Enough. macOS forgets where your windows go. redock doesn't. Not the screen, not the spot, and not which desktop (Space) each window lives on.

## TL;DR

```sh
save-windows office      # before you unplug
restore-windows office   # after you plug back in. Done.
```

## Get it

Just two files. No app, no installer:

```sh
mkdir -p ~/bin
for f in save-windows restore-windows; do
  curl -fsSL "https://raw.githubusercontent.com/drxyu/redock/main/$f" -o ~/bin/$f && chmod +x ~/bin/$f
done
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.zshrc   # skip if ~/bin is already on your PATH
```

Open a new terminal.

## First run: say yes to macOS

macOS won't let anything move your windows until you allow it:

- **Accessibility**: System Settings > Privacy & Security > Accessibility. Add your terminal app, turn it on, then quit and reopen the terminal.
- **Automation**: if macOS asks whether your terminal may control "System Events", click OK. Clicked Don't Allow? Turn it on under Privacy & Security > Automation.
- **Mission Control shortcuts**: "Move left a space" and "Move right a space" must be on (they are by default) under System Settings > Keyboard > Keyboard Shortcuts > Mission Control.

If something is missing, the commands tell you what.

## Desktops

macOS only lets a program see the windows on the current desktop, so both commands flip through every desktop that has windows and come back when done. To carry a window to another desktop, restore holds it by its title bar while switching desktops, the only way macOS still allows. You will see desktops change and the pointer jump for a second per window. Keep your hands off the keyboard and mouse until it reports back.

## Commands

```sh
save-windows home        # save every window's spot as "home"
save-windows             # same, under the tag "default" (the old default is kept as default.bak-<date>)
save-windows -l          # list your saved layouts
save-windows -l home     # show what "home" remembers
save-windows -d home     # delete "home"
restore-windows home     # put everything back
restore-windows          # pick a layout from a menu
```

A window redock can't identify for sure gets skipped, never shuffled around. Minimized windows and full screen apps stay where they are.

## Fine print

Reading which desktop a window is on uses private macOS APIs, so a macOS update may break it. Not yet tested with "Displays have separate Spaces" turned on. Provided as is, no warranty. Use at your own risk. MIT licensed, see [LICENSE](LICENSE). Made by [drxyu](https://github.com/drxyu).
