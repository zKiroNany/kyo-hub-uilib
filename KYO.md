# Kyo skin notes

This repo is a fork of [deividcomsono/Obsidian](https://github.com/deividcomsono/Obsidian) (MIT).
All Kyo changes are marked with `--// Kyo:` comments in `Library.lua` so upstream merges stay easy.

Base snapshot: upstream `bdaa9b8` (first commit of this repo).

## Skin patches (Sea Hub look)

- `Library.Scheme` defaults: Sea Hub palette (background 28, main 35, outline 50, accent 131/181/255), GothamMedium font.
- `Templates.Window` defaults: size 660x460, corner radius 6, new `SidebarWidth = 176`, animations on, tab buttons with accent indicator.
- Window: accent border, left aligned accent title, sidebar and page as floating rounded panels (`PanelMargin`).
- Tabs: 34px buttons, outline-colored selected background at 0.5 transparency.
- Groupbox: accent title and accent underline (`Library:MakeLine` accepts an optional `Color`).

## Untouched on purpose

SaveManager, flags (`Options` / `Toggles`), keybinds, mobile handling and the unload/cleanup system. ThemeManager only had its Default theme and font default changed (see below).

## Loading the fork

`Example.lua` and the asset downloader (`BaseURL` in `Library.lua`) point at this repo. Always load `Library.lua`, `addons/ThemeManager.lua` and `addons/SaveManager.lua` from the same Kyo URL: the upstream `ThemeManager` would overwrite the skin with its own Default theme.

`addons/ThemeManager.lua`: the `Default` built-in theme uses the Sea Hub palette and the font dropdown defaults to GothamMedium.
