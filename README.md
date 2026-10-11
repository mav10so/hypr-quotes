# hypr-quotes

A **quote of the day** for your Hyprland desktop. A short, lean quote is composited
onto your wallpaper, chosen deterministically by **day-of-year** (same all day,
cycles once per year) and rotated automatically at midnight. It sits behind your
windows, just like a wallpaper. On [Omarchy](https://omarchy.org) it follows
whatever background you pick.

## How it works

- **`quote-of-the-day`** prints the day's line from `quotes.txt` (by day-of-year).
  Call it from your shell greeting too, and the terminal shows the same quote.
- **`wallpaper-quote`** composites that line onto a base image with ImageMagick,
  sized to your largest monitor → `~/.cache/hypr/wallpaper-<date>-<time>.jpg`.
- **`wallpaper-quote-daemon`** applies it at login and again after each local
  midnight (also after a resume from suspend). Singleton-locked.
  - **Omarchy:** applied with `omarchy-theme-bg-set`. When you pick another
    background (background switcher or theme change), that image becomes the new
    base and gets the quote within a couple of seconds, and it stays the base
    across days and reboots.
  - **Plain Hyprland:** applied through `hyprpaper`'s IPC if it is running,
    otherwise with `swaybg`.

## Requirements

- [Hyprland](https://hypr.land), with Omarchy, `hyprpaper` or `swaybg`
- ImageMagick (`magick`) and `jq`
- A font (default: JetBrainsMono Nerd Font — change `FONT` in `wallpaper-quote`)

## Install

```sh
install -Dm755 quote-of-the-day       ~/.local/bin/quote-of-the-day
install -Dm755 wallpaper-quote        ~/.local/bin/wallpaper-quote
install -Dm755 wallpaper-quote-daemon ~/.local/bin/wallpaper-quote-daemon
install -Dm644 quotes.txt             ~/.config/hypr/quotes.txt
```

1. Set your **font** at the top of `wallpaper-quote` (`FONT`). Also set
   **`DEFAULT_BASE`**, the base image used until you pick a background in Omarchy
   (or always, without Omarchy).
2. Start the daemon at login:

   - **Omarchy** — `~/.config/hypr/autostart.lua`:

     ```lua
     o.launch_on_start("$HOME/.local/bin/wallpaper-quote-daemon")
     ```

   - **Plain Hyprland** — run `hyprpaper` (with `splash = false` in
     `hyprpaper.conf`) or install `swaybg`, then in your Hyprland config:

     ```ini
     exec-once = ~/.local/bin/wallpaper-quote-daemon
     ```

     If no wallpaper backend is up yet when the daemon starts, it retries every
     5 minutes.

## Customize

- **Quotes:** edit `quotes.txt` — one per line, `#` lines and blanks ignored.
  **Line N = day N of the year**, so reordering re-arranges the calendar.
- **Style:** `POINTSIZE`, `BOTTOM_OFFSET`, `SHADOW`, `TEXT_OPACITY`, `FONT` at the
  top of `wallpaper-quote`. Sizes are pixels on a 1440px-tall screen and scale
  with your resolution.

## Known limitation

On Omarchy, `omarchy theme bg next` always jumps to the theme's first background:
it works out "next" from the current background, which is the quote render. The
background switcher (and theme changes) work normally.

## Quotes

Hand-picked lines (books, film, poetry, builders, leaders) blended with a short,
well-attributed subset of the open [quotable.io dataset](https://github.com/quotable-io/data)
(MIT). Swap in your own freely.

## License

MIT — see [LICENSE](LICENSE).
