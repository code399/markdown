## DWM 安装与配置

---

### 安装字体

```
sudo pacman -S adobe-source-han-serif-cn-fonts wqy-microhei
sudo pacman -S noto-fonts-cjk noto-fonts-emoji noto-fonts-extra
sudo pacman -S nerd-fonts-dejavu-sans-mono
sudo pacman -S nerd-fonts-jetbrains-mono
```

### 安装 intel 核显驱动

```
sudo pacman -S xf86-video-intel mesa-amber lib32-mesa-amber vulkan-intel lib32-vulkan-intel
```

### 安装声卡驱动

```
sudo pacman -S alsa-utils
```

### 安装 X 窗口系统

```
sudo pacman -S xorg xorg-xinit
```

### 安装必要程序

```
sudo pacman -S picom
```

### 安装 DWM

```
sudo pacman -S git

git clone https://git.suckless.org/dwm --depth=1
rm -rf dwm/.git
sudo make clean install

git clone https://git.suckless.org/st --depth=1
rm -rf st/.git
sudo make clean install

git clone https://git.suckless.org/dmenu --depth=1
rm -rf dmenu/.git
sudo make clean install

git clone https://git.suckless.org/slstatus
rm -rf slstatus/.git
sudo make clean install
```

### 配置 DWM

```
cp /etc/X11/xinit/xinitrc ~/.xinitrc
sudo nvim ~/.xinitrc
```

添加

```
export LANG=zh_CN.UTF-8
export LC_CTYPE=zh_CN.UTF-8
picom -CGb &
exec slstatus &
exec dwm
```

注：至少要确保 `/etc/X11/xinit/xinitrc` 中的最后一个 `if` 块出现在您的 `~/.xinitrc` 文件中。

`~/dwm/config.h`

```c
static const char *fonts[] = { "JetBrainsMono Nerd Font Mono:style=medium:size=13", "WenQuanYi Micro Hei:style=Regular:size=13:antialias=true:autohint=true" };
static const char dmenufont[] = "JetBrainsMono Nerd Font Mono:size=13";

static const char *tags[] = {" ", " ", " ", " ", " ", " ", " ", " ", "    " };
static const char *dmenucmd[] = { "rofi", "-show", "drun", NULL };
```

`~/st/config.h`

```c
static char *font = "JetBrainsMono Nerd Font :pixelsize=15:antialias=true:autohint=true";
```

`~/slstatus/config.h`

```c
{ run_command,	"  %s |",		"uname -r | awk -F \"-\" '{ print $1 }'"},
{ disk_free,	"  %s |",		"/" },
{ cpu_perc,		"  %s%% |",	NULL},
{ ram_perc,		"  %s%% |",		NULL},
{ run_command,	"  %s |",		"amixer sget Master | awk -F \"[][]\" 'NF>1 { print $2 }'"},
{ run_command,	" %s |",		"amixer sget Capture | awk -F \"[][]\" '/Left:/ { print $2 }'"},
{ wifi_perc,	"󰖩  %s%% |",	"wlan0"},
{ datetime,	"%s",	"%y-%m-%d %T %a" },
```



### 安装文件浏览器

```
sudo pacman -S ntfs-3g udisks2 udiskie pcmanfm
sysytemctl enable udisks2
sudo nvim ~/.xinitrc
```

添加

```
udiskie &
```

### 设置壁纸

```
sudo pacman -S feh
sudo nvim ~/.xinitrc
```

添加（显示指定壁纸）

```
feh --bg-fill 图片绝对目录/图片名
```

添加（随机显示壁纸）

```
feh --bg-fill --randomize 图片绝对目录/*
```

### 中文输入法

```
sudo pacman -S fcitx5-im fcitx5-configtool
sudo pacman -S fcitx5-chinese-addons
sudo pacman -S fcitx5-material-color
```

离线字库

```
sudo pacman -S fcitx5-pinyin-zhwiki fcitx5-pinyin-moegirl
```



`/etc/environment`

```
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
```

`~/.xinitrc`

```
fcitx5 &
```

运行 `fcitx5-configtool` 配置输入法。

### 安装 V2raya

```
sudo pacman -S v2raya
sudo systemctl enable --now v2raya.service
```

浏览器访问 http://localhost:2017/

### ST 补丁

```
patch < patch.diff
```

st-alpha

st-anysize



| 默认按键                   | 我的按键              | 实现功能                     |
| -------------------------- | --------------------- | ---------------------------- |
| Shift+Alt+Enter            | Alt+Enter             | 打开终端                     |
| Alt+b                      | Super+s               | 开启/关闭状态栏              |
| Alt+p                      | Alt+r                 | rofi ( dmemu )               |
| [Mod]+[Enter]              | [Mod]+[Space]         | 使窗口在主窗口和栈窗口间切换 |
| [Mod] + [j / k]            | [Mod] + [j / k]       | 选择上一下或下一个窗口       |
| [Mod] + [h / l]            | [Mod] + [h / l]       | 加大或缩小当前窗口显示范围   |
| [Mod]+[2]                  | [Mod]+[2]             | 切换到2号标签                |
| [Shift]+[Mod]+[2]          | [Shift]+[Mod]+[2]     | 将当前窗口放到2号标签        |
| [Mod] + [i / d]            | [Mod] + [i / d]       | 增加或减少主窗口的窗口个数   |
| [Mod] + [, / .]            | [Mod] + [, / .]       | 多屏幕间切换                 |
| [Shift]+[Mod]+[, / .]      | [Shift]+[Mod]+[, / .] | 将当前窗口放到另一个屏幕上   |
| [Mod]+[0]                  | [Mod]+[0]             | 在当前屏幕上显示所有窗口     |
| [Shift]+[Mod]+[0]          | [Shift]+[Mod]+[0]     | 将当前窗口放到所有标签       |
| Shift+Alt+c                | Super+q               | 关闭当前窗口                 |
| Shift+Alt+q                | Super+ESC             | 退出 dwm                     |
| Alt+g                      | Super+g               | 网格模式                     |
| Alt+t                      | Super+t               | 平铺模式                     |
| Alt+f                      | Super+f               | 浮动模式                     |
| Alt+m                      | Super+m               | 全屏模式                     |
| 下面是有关浮动模式的快捷键 |                       |                              |
|                            |                       | 调整浮动窗口大小             |
| [Mod]+[L M B]              |                       | 移动浮动窗口                 |
| [Mod]+[Space]              |                       | 切换布局模式                 |
| [Mod]+[R M B]              |                       | 让窗口浮动                   |
| [Mod]+[M M B]              |                       | 让窗口不要浮动了             |
