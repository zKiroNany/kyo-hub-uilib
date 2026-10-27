# Kyo skin notes

This repo is a fork of [deividcomsono/Obsidian](https://github.com/deividcomsono/Obsidian) (MIT).
All Kyo changes are marked with `--// Kyo:` comments in `Library.lua` so upstream merges stay easy.

Base snapshot: upstream `bdaa9b8` (first commit of this repo).

## Skin patches (Sea Hub look)

- `Library.Scheme` defaults match the Default theme: dark purple (`170f1c` background, `331a47` main, `4b316b` outline), mint accent `a4ff8e` with a `bc7df4` gradient, white text, ArimoBold body font, GothamBlack headers (the Default theme itself uses Bungee for headers, downloaded on first load, GothamBlack is the fallback until it arrives).
- Buttons and sub buttons: accent fill (0.1 transparency, 0 on hover), white label, outline stroke only when disabled.
- Sliders (non compact): thin 8px bar, value at the right of the title row. Compact sliders keep the Obsidian look.
- `Library:SetRainbowAccent(Enabled, Speed)`: cycles `AccentColor` at 30 fps through the registry, disconnected by `Unload`. Restores the previous accent when turned off.
- `Templates.Window` defaults: size 660x460, corner radius 6, new `SidebarWidth = 176`, animations on, tab buttons with accent indicator.
- Window: accent border, left aligned accent title, sidebar and page as floating rounded panels (`PanelMargin`).
- Tabs: 34px buttons, outline-colored selected background at 0.5 transparency.
- Groupbox: accent title and accent underline (`Library:MakeLine` accepts an optional `Color`).
- Footer bar (footer text and resize handle) has no background, the window or background image shows through.
- ThemeManager's "Themes" groupbox starts collapsed on every load (`Collapsed = true`, and `Groupbox.AlwaysCollapsed` keeps SaveManager layouts from reopening it).

## Fonts, Bold and gradient

- `Library.Scheme.HeaderFont` (default GothamBold) is used by tab labels, groupbox titles and the window title. Body text uses `Library.Scheme.Font`. `Library:SetHeaderFont(Enum.Font.X)` / `Library:SetFont(Enum.Font.X)`.
- `Bold = true` in the element info uses the header font for that element title (Toggle, Checkbox, Label, Button, SubButton, Slider, Input, Dropdown). `Library:SetBold(TextObject, true)` does the same on any text.
- ThemeManager: font dropdowns list every `Enum.Font` (searchable), plus "Header Font (Bold)", "Gradient color" and "Accent Gradient". They are saved in themes / theme JSON and ignored by SaveManager configs.
- Accent gradient (default mint `a4ff8e` to purple `bc7df4`): `Library:SetGradient(Enabled, Color2)`. Applied to window border and title, groupbox title and line, tab indicator and icons, buttons, slider fill and toggle switch when on. Themes without a second color get an automatic one (`Library:GetAutoGradientColor`). Add it to your own element with `Library:AddGradient(Instance, "BackgroundColor3", Rotation)`, and call `Library:SyncGradient(Instance)` after changing its registry key by hand.
- Custom fonts: `Library.CustomFonts` maps a display name to a TTF in `fonts/` (Montserrat, Poppins, Monocraft (Minecraft), Press Start 2P, Silkscreen, Jersey 10, VT323, Audiowide, Russo One, Righteous, Bungee, Chakra Petch, Rajdhani; all SIL OFL, licenses in `fonts/licenses`). `Library:SetFont("Montserrat")` / `SetHeaderFont` take an `Enum.Font` name or a custom name. The file is downloaded once to `Obsidian/custom_fonts`, registered as a font family with `getcustomasset` and cached, so it needs an executor with `writefile` + `getcustomasset`; if it fails a notification explains why and the old font stays. The names also show up in the ThemeManager font dropdowns. Add more with `Library.CustomFonts["Name"] = "file.ttf"` after uploading the TTF to `fonts/`.
- Tab icons: `IconColor = Color3` in `AddTab` / `AddKeyTab` (or 5th positional argument, or `Tab:SetIconColor(Color)`) gives that icon its own color with an automatic gradient. Without it the icon follows the accent gradient.

- Background image: lives inside the window behind every element (cropped, not stretched). While active, sidebar, page and groupboxes become semi transparent (`Library.BackgroundPanelTransparency`, default 0.35). PNG / JPG / BMP only, failures are reported with a notification. `Library:SetBackgroundImageTransparency` / ThemeManager "Background Opacity" slider control the image opacity. Add your own panels with `Library:AddPanelSurface(Frame)`.
- Animated background: a link to a `.gif` is downloaded and decoded in Lua (all frames, transparency, disposal and interlace handled, checked against Pillow output) and played through an `EditableImage` (needs executor support). Big GIFs take a few seconds to decode, keep them under ~500 px and ~100 frames. A `.webm` / `.mp4` link plays as a muted looping video instead (needs `getcustomasset` video support).

- Favorites: `Favorites = "Key"` in any `AddDropdown`. Right click (or hold a finger for `Library.FavoriteHoldTime`, default 0.6 s, without moving) on an item toggles its star. Favorites are pinned to the top and saved in `<ThemeManager folder>/favorites.json` (`Library:SetFavoritesPath`). Enabled on the theme lists, font lists and the config list. API: `Dropdown:IsFavorite(Value)`, `Dropdown:SetFavorite(Value, Bool)`.
- Hover tweens on buttons, tabs and key pickers are skipped while the window fades (`Library.WindowFading`), otherwise the hovered element stayed visible until the fade ended.

- Readable button text: the label and icon on accent buttons use the theme font color when it reads at 4.5:1 on every color of the accent gradient, otherwise white or near black (`Library:GetAccentLabelColor`, `Library:GetLabelColor(Surfaces, Preferred)`, `Library:AddAccentText`). Themes without a second accent get an automatic gradient color that is pulled toward the accent until the text stays readable (`Library:GetAutoGradientColor(Color, true)`). The ThemeManager "Contrast check" now also measures button text against the accent gradient, and every built-in theme passes it (checked by running the Lua helpers on all 25 built-ins). The "Are you sure?" label of double click buttons no longer turns accent colored (it was invisible on the accent fill).
- Themes: Default (mint accent, `backgrounds/default.jpg`, a purple wave from Vecteezy, free license, keep the credit "Image from Vecteezy") and Mint Purple (same colors with an original mint to purple wave, `backgrounds/mint-purple.jpg`), plus 8 community themes (Tokyo Ghoul, One Piece, Halloween, Witch Brew, Naruto, Demon Slayer, Jujutsu Kaisen, Chainsaw Man) with their own colors, gradient, header font and background image (`backgrounds/<theme>.jpg`, 756x504 original AI generated artwork made for Kyo, no characters, logos or text, loaded from this repo through `BackgroundBaseURL` in ThemeManager). Choosing any of the old built-in themes clears the background again. Themes apply their background without the "applied" popup, and the same link is never downloaded twice in a row. Default background opacity is 50%. Built-in themes that do not set fonts go back to GothamMedium / GothamBold. To change an image, upload it under a new file name, files are cached on disk by URL.

## Untouched on purpose

SaveManager, flags (`Options` / `Toggles`), keybinds, mobile handling and the unload/cleanup system. ThemeManager only had its Default theme and font default changed (see below).

## Loading the fork

`Example.lua` and the asset downloader (`BaseURL` in `Library.lua`) point at this repo. Always load `Library.lua`, `addons/ThemeManager.lua` and `addons/SaveManager.lua` from the same Kyo URL: the upstream `ThemeManager` would overwrite the skin with its own Default theme.

`addons/ThemeManager.lua`: the `Default` built-in theme uses the Sea Hub palette and the font dropdown defaults to GothamMedium.
