# redock

**Listen up, Mac users with more than one monitor.**

You know the ritual. You yank the cable and run to a meeting. You come back, plug it all in again, and macOS greets you with a crime scene: every window you own crammed onto one screen, piled up like laundry. Slack is sitting on your editor. Your editor is sitting on three browsers. Your terminals are... somewhere.

So you drag. Window. By. Window. Back. Into. Place.

Every. Single. Day.

**Enough.** macOS forgot where your windows go. redock didn't.

```sh
save-windows office      # before you unplug
restore-windows office   # after you plug back in. Boom. Done.
```

That's the whole trick. Every window jumps back to the screen it came from, at the exact size and spot you left it.

## Why you'll love it (besides the hours of your life you get back)

- **Zero apps. Zero menu-bar clutter. Zero subscriptions.** Two tiny shell scripts running on stuff your Mac already has. No "Pro" tier. No trial that expires on a Tuesday.
- **It knows which Chrome window is which.** redock tracks the window numbers macOS itself hands out, so your three Chrome windows stop playing musical chairs.
- **It doesn't guess.** Can't tell for sure which window is which? It says so and leaves that window alone. No random reshuffles, ever.
- **One layout per desk.** `home`, `office`, `that-coffee-shop-with-the-ultrawide`. Save them all, restore whichever one you're sitting at.
- **Too lazy to type a name?** Respect. Run `restore-windows` and pick one from a menu with the arrow keys.

## Get it (30 seconds, tops)

```sh
git clone https://github.com/drxyu/redock.git ~/redock
echo 'export PATH="$HOME/redock:$PATH"' >> ~/.zshrc
```

Open a new terminal. The first time, macOS will ask for Accessibility permission for your terminal app (fair enough, we're about to fling its windows around): System Settings > Privacy & Security > Accessibility. Add your terminal, flip it on, then quit and reopen it.

What you need: macOS, its built-in `osascript` and `/bin/bash`. Nothing to brew. Nothing to npm. Nothing to "just quickly configure".

## The entire manual

```sh
save-windows home        # save every window's place under the tag "home"
save-windows             # list saved tags, newest first, with date and window count
save-windows -l          # same as above
save-windows -l home     # show exactly which windows "home" remembers
save-windows -d office   # forget the tag "office"

restore-windows home     # put everything back the way "home" remembers it
restore-windows          # pick a tag from a menu
```

In the menu: up/down or j/k to move, Enter to restore, q or Esc to bail. Every other key is ignored, so a stray keypress won't fire off a restore.

Tags take letters, digits, dot, dash and underscore, and start with a letter or digit. Saving to a tag that already exists overwrites it. No "are you sure?" pop-up, you're an adult.

## How the magic works (spoiler: it's not magic)

- `save-windows` writes down each window's app, title, position, size and the window number macOS gave it.
- `restore-windows` hunts each window down by that number, moves it, and checks the move actually landed on that exact window. Closed and reopened a window since you saved? Then it's matched by exact title only. Anything it can't pin down is reported as not placed and left right where it is.
- Both commands print one line per window, so you see exactly what got saved, moved or skipped. No black box.

## Where your layouts live

- Each layout is one small file: `~/.window-layouts/<tag>.json`. Set `WINDOW_LAYOUT_DIR` to keep them somewhere else.
- Still have an old single layout at `~/.window-layout.json`? It gets moved to the tag `default` the first time either command runs. Nothing lost.

## When things go sideways

- **"Accessibility permission is missing"**: give your terminal app the permission described under Get it, then quit and reopen the app. Yes, fully quit.
- **Some windows "not placed"**: they were closed, or their title changed since you saved. Save again while they're open.
- **A window lands somewhere weird**: your displays are probably arranged differently than when you saved. Save one tag per arrangement and stop fighting it.

## The fine print (yes, we have to)

redock moves and resizes windows on your screen through macOS Accessibility. It comes as is, without warranty of any kind. You use it at your own risk. The author is not responsible for lost work, misplaced windows, data loss or any other damage arising from its use. Read the scripts before you run them (they're short, promise), and save your work first.

Made by [drxyu](https://github.com/drxyu). MIT licensed, full legalese in [LICENSE](LICENSE).
