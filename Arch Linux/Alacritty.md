## Alacritty

---

[alacritty](https://github.com/alacritty/alacritty)

[alacritty-theme](https://github.com/eendroroy/alacritty-theme)

```
sudo pacman -S alacritty
cd ~/.config
mkdir alacritty
cd alacritty
sudo nvim alacritty.yml
```

~/.config/alacritty/alacritty.yml

```yaml
# Colors (One Dark)
colors:
  # Default colors
  primary:
    background: '0x1e2127'
    foreground: '0xabb2bf'

  # Normal colors
  normal:
    black:   '0x1e2127'
    red:     '0xe06c75'
    green:   '0x98c379'
    yellow:  '0xd19a66'
    blue:    '0x61afef'
    magenta: '0xc678dd'
    cyan:    '0x56b6c2'
    white:   '0xabb2bf'

  # Bright colors
  bright:
    black:   '0x5c6370'
    red:     '0xe06c75'
    green:   '0x98c379'
    yellow:  '0xd19a66'
    blue:    '0x61afef'
    magenta: '0xc678dd'
    cyan:    '0x56b6c2'
    white:   '0xffffff'

# set font
font:
  normal:
    family: "JetBrainsMono Nerd Font Mono"
    style: Regular
  bold:
    family: "JetBrainsMono Nerd Font Mono"
    style: Bold
  italic:
    family: "JetBrainsMono Nerd Font Mono"
    style: Italic
  bold_italic:
    family: "JetBrainsMono Nerd Font Mono"
    style: Bold Italic
  size: 12.0 
  offset:
    x: 0
    y: 0
  glyph_offset:
    x: 0
    y: 0

cursor:
  style:
    shape: Underline
    blinking: On
  blink_timeout: 0
  blink_interwal: 200

window:
  padding:
    x: 2
    y: 2
  opacity: 0.9

scrolling:
  history: 5000
  multiplier: 5

draw_bold_text_with_bright_colors: true

selection:
  semantic_escape_chars: ",│`|:\"' ()[]{}<>\t"
  save_to_clipboard: true

live_config_reload: true

key_bindings:
  - { key: V, mods: command, action: Paste }
  - { key: C, mods: command, action: Copy }
```

