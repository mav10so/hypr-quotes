# hypr-quotes

A **quote of the day** for your Hyprland desktop. A short, lean quote is composited
onto your wallpaper — replacing Hyprland's built-in splash — chosen deterministically
by **day-of-year** (same all day, cycles once per year) and rotated automatically at
midnight. It sits behind your windows, just like a wallpaper.

## How it works

- **`wallpaper-quote`** picks the day's line from `quotes.txt` (by day-of-year) and
  composites it onto a base image with ImageMagick → `~/.cache/hypr/wallpaper.jpg`.
- **`wallpaper-quote-daemon`** applies that image now, then regenerates + reapplies
  it at each local midnight, so the quote advances even if you never log out.
  (Singleton-locked.) It auto-detects how to set the wallpaper: **Omarchy**'s native
  `omarchy-theme-bg-set` if present, otherwise **hyprpaper** (kill + relaunch — IPC
  reload is unreliable in practice), otherwise **swaybg**.

## Requirements

- [Hyprland](https://hypr.land) — with either [Omarchy](https://omarchy.org),
  `hyprpaper`, or `swaybg` for displaying the wallpaper
- ImageMagick (`magick`)
- A font (default: JetBrainsMono Nerd Font — change `FONT` in `wallpaper-quote`)

## Install

```sh
install -Dm755 wallpaper-quote        ~/.local/bin/wallpaper-quote
install -Dm755 wallpaper-quote-daemon ~/.local/bin/wallpaper-quote-daemon
install -Dm644 quotes.txt             ~/.config/hypr/quotes.txt
```

1. Set your **base image** and **font** at the top of `wallpaper-quote` (`BASE`, `FONT`).
2. **On Omarchy:** nothing else to configure — `omarchy-theme-bg-set` is used
   automatically. **On plain Hyprland with hyprpaper:** point it at the generated
   image and disable its splash — `~/.config/hypr/hyprpaper.conf` (hyprpaper needs
   an **absolute** path, so write out your home directory):

   ```ini
   splash = false

   wallpaper {
       monitor  =
       path     = /home/YOU/.cache/hypr/wallpaper.jpg
       fit_mode = cover
   }
   ```

3. Start the daemon at login — in your Hyprland config. It applies the current
   day's quote immediately, then keeps it fresh at midnight:

   ```ini
   exec-once = ~/.local/bin/wallpaper-quote-daemon
   ```

## Customize

- **Quotes:** edit `quotes.txt` — one per line, `#` lines and blanks ignored.
  **Line N = day N of the year**, so reordering re-arranges the calendar.
- **Style:** `POINTSIZE`, `BOTTOM_OFFSET`, `TEXT_OPACITY`, `FONT` at the top of `wallpaper-quote`.

## Quotes

Hand-picked lines (books, film, poetry, builders, leaders) blended with a short,
well-attributed subset of the open [quotable.io dataset](https://github.com/quotable-io/data)
(MIT). Swap in your own freely.

## License

MIT — see [LICENSE](LICENSE).
