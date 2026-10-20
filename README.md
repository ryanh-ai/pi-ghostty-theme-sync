# pi-ghostty-theme-sync

Sync [pi](https://github.com/badlogic/pi-mono) theme with your active [Ghostty](https://ghostty.org/) terminal colors.

![demo](https://raw.githubusercontent.com/ogulcancelik/pi-ghostty-theme-sync/main/assets/demo.gif)

On startup, the extension runs `ghostty +show-config`, generates a pi theme, and switches pi to it. On macOS, it also watches the system appearance and updates Pi within about two seconds when light or dark mode changes.

## install

```bash
pi install npm:pi-ghostty-theme-sync
```

(Optional, try without installing):

```bash
pi -e npm:pi-ghostty-theme-sync
```

## how it works

- Reads: `background`, `foreground`, and `palette[0..15]` from Ghostty.
- Computes a hash of `bg/fg + palette[0..15]`.
- Writes a theme to: `~/.pi/agent/themes/ghostty-sync-<hash>.json`.
- Removes older `ghostty-sync-*.json` files (keeps only the current one).
- Sets pi theme to `ghostty-sync-<hash>`.
- In interactive macOS sessions, polls system appearance every two seconds and regenerates the active theme when light/dark mode changes.

## mapping notes

Uses ANSI slot semantics:

- `palette[1]` → `error`
- `palette[2]` → `success`
- `palette[3]` → `warning`
- `palette[4]` → `link` (links/borders)
- `palette[5]` → `accent` (primary highlight)
- `palette[6]` → `accentAlt` (secondary highlight)

Neutrals (`muted`, `dim`, subtle borders) are derived from `background` + `foreground` instead of trusting `palette[0]`/`palette[8]`.

Text colors are contrast-checked (WCAG) against every generated Pi background: body text targets 5.5:1, muted text 4.5:1, and dim text 3.2:1. Body text is emitted as an explicit color rather than the terminal default, so terminals with a minimum-contrast setting (for example Ghostty's `minimum-contrast = 4`) do not snap text in tinted tool boxes to pure black or white.

## requirements

- Ghostty installed and `ghostty` available in `PATH`

## license

MIT
