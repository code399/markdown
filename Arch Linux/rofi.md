## rofi

---

```
sudo pacman -S rofi papirus-icon-theme
git clone https://github.com/lr-tech/rofi-themes-collection.git
cd rofi-themes-collection
mkdir -p ~/.local/share/rofi/themes/
cp -r themes/. ~/.local/share/rofi/themes/
rofi -show run
```

在 rofi 菜单中运行 rofi-theme-selector，选择主题，回车预览，Alt+a 选定。

rofi 还会在 /usr/share/rofi/themes 中搜索主题。

`~/.config/rofi/config.rasi`

```
configuration {
  modi: "drun,run,filebrowser,window";
  show-icons: true;
  icon-theme: "Papirus";
}
```

