# Kyo skin notes

This repo is a fork of [deividcomsono/Obsidian](https://github.com/deividcomsono/Obsidian) (MIT).
All Kyo changes are marked with `--// Kyo:` comments in `Library.lua` so upstream merges stay easy.

Base snapshot: upstream `bdaa9b8` (first commit of this repo).

## Skin patches (Sea Hub look)

- `Library.Scheme` defaults: Sea Hub grey palette (background 28, main 35, outline 50) with Kyo purple accent (143/72/207, `8f48cf`), GothamMedium font.
- Buttons and sub buttons: accent fill (0.1 transparency, 0 on hover), white label, outline stroke only when disabled.
- Sliders (non compact): thin 8px bar, value at the right of the title row. Compact sliders keep the Obsidian look.
- `Library:SetRainbowAccent(Enabled, Speed)`: cycles `AccentColor` at 30 fps through the registry, disconnected by `Unload`. Restores the previous accent when turned off.
- `Templates.Window` defaults: size 660x460, corner radius 6, new `SidebarWidth = 176`, animations on, tab buttons with accent indicator.
- Window: accent border, left aligned accent title, sidebar and page as floating rounded panels (`PanelMargin`).
- Tabs: 34px buttons, outline-colored selected background at 0.5 transparency.
- Groupbox: accent title and accent underline (`Library:MakeLine` accepts an optional `Color`).

## Fonts, Bold and gradient

- `Library.Scheme.HeaderFont` (default GothamBold) is used by tab labels, groupbox titles and the window title. Body text uses `Library.Scheme.Font`. `Library:SetHeaderFont(Enum.Font.X)` / `Library:SetFont(Enum.Font.X)`.
- `Bold = true` in the element info uses the header font for that element title (Toggle, Checkbox, Label, Button, SubButton, Slider, Input, Dropdown). `Library:SetBold(TextObject, true)` does the same on any text.
- ThemeManager: font dropdowns list every `Enum.Font` (searchable), plus "Header Font (Bold)", "Gradient color" and "Accent Gradient". They are saved in themes / theme JSON and ignored by SaveManager configs.
- Accent gradient (default purple `8f48cf` to pink `ec4899`): `Library:SetGradient(Enabled, Color2)`. Applied to window border and title, groupbox title and line, tab indicator and icons, buttons, slider fill and toggle switch when on. Themes without a second color get an automatic one (`Library:GetAutoGradientColor`). Add it to your own element with `Library:AddGradient(Instance, "BackgroundColor3", Rotation)`, and call `Library:SyncGradient(Instance)` after changing its registry key by hand.
- Tab icons: `IconColor = Color3` in `AddTab` / `AddKeyTab` (or 5th positional argument, or `Tab:SetIconColor(Color)`) gives that icon its own color with an automatic gradient. Without it the icon follows the accent gradient.

- Background image: lives inside the window behind every element (cropped, not stretched). While active, sidebar, page and groupboxes become semi transparent (`Library.BackgroundPanelTransparency`, default 0.35). PNG / JPG / BMP only, failures are reported with a notification. `Library:SetBackgroundImageTransparency` / ThemeManager "Background Opacity" slider control the image opacity. Add your own panels with `Library:AddPanelSurface(Frame)`.
- Animated background: a link to a `.gif` is downloaded and decoded in Lua (all frames, transparency, disposal and interlace handled, checked against Pillow output) and played through an `EditableImage` (needs executor support). Big GIFs take a few seconds to decode, keep them under ~500 px and ~100 frames. A `.webm` / `.mp4` link plays as a muted looping video instead (needs `getcustomasset` video support).

- Favorites: `Favorites = "Key"` in any `AddDropdown`. Right click (or hold a finger for `Library.FavoriteHoldTime`, default 0.6 s, without moving) on an item toggles its star. Favorites are pinned to the top and saved in `<ThemeManager folder>/favorites.json` (`Library:SetFavoritesPath`). Enabled on the theme lists, font lists and the config list. API: `Dropdown:IsFavorite(Value)`, `Dropdown:SetFavorite(Value, Bool)`.
- Hover tweens on buttons, tabs and key pickers are skipped while the window fades (`Library.WindowFading`), otherwise the hovered element stayed visible until the fade ended.

- Themes: Default plus 8 community themes (Tokyo Ghoul, One Piece, Halloween, Witch Brew, Naruto, Demon Slayer, Jujutsu Kaisen, Chainsaw Man) with their own colors, gradient and header font. They never touch the background image. Built-in themes that do not set fonts go back to GothamMedium / GothamBold.

## Untouched on purpose

SaveManager, flags (`Options` / `Toggles`), keybinds, mobile handling and the unload/cleanup system. ThemeManager only had its Default theme and font default changed (see below).

## Loading the fork

`Example.lua` and the asset downloader (`BaseURL` in `Library.lua`) point at this repo. Always load `Library.lua`, `addons/ThemeManager.lua` and `addons/SaveManager.lua` from the same Kyo URL: the upstream `ThemeManager` would overwrite the skin with its own Default theme.

`addons/ThemeManager.lua`: the `Default` built-in theme uses the Sea Hub palette and the font dropdown defaults to GothamMedium.
