## 配置 dwm

---

### 设置字体

- 下载字体

```
sudo pacman -S wqy-zenhei wqy-microhei
sudo pacman -S noto-fonts-cjk noto-fonts-emoji noto-fonts-extra
sudo pacman -S nerd-fonts-jetbrains-mono
```

- 设置字体

分别编辑 dwm 与 st 目录下的 `config.h` 文件

```
nvim config.h
```

`~/dwm/config.h`

```c
static const char *fonts[] = { "JetBrainsMono Nerd Font:style=medium:size=13", "WenQuanYi Micro Hei:style=Regular:size=13:antialias=true:autohint=true" };
static const char dmenufont[] = "JetBrainsMono Nerd Font:style=medium:size=13";

static const char *tags[] = {" ", " ", " ", " ", " ", " ", " ", " ", " " };
```

- ``  `` ` ` `󰍮` `` `` 

`~/st/config.h`

```c
static char *font = "JetBrainsMono Nerd Font Mono:style=Regular:size=15:antialias=true:autohint=true";
```

注：若更改的是 `config.def.h` 文件，则需删除当前存在的 `config.h` 文件，然后进行编译安装。

### 安装文件浏览器

```
sudo pacman -S ntfs-3g udisks2 udiskie pcmanfm
sysytemctl enable udisks2
sudo nvim ~/.xinitrc

# 添加如下
udiskie &
```

### 设置壁纸

```
sudo pacman -S feh
sudo nvim ~/.xinitrc

# 添加如下
feh --bg-fill .../图片名 # 显示指定壁纸
feh --bg-fill --randomize .../* # 随机显示壁纸
```

### 配置状态栏

下载 slstatus

```
git clone https://git.suckless.org/slstatus
sudo nvim ~/.xinitrc

# 添加如下
exec slstatus &

nvim config.h

{ run_command,	"  %s |",		"uname -r | awk -F \"-\" '{ print $1 }'"},
{ disk_free,	"  %s |",		"/" },
{ cpu_perc,		"  %s%% |",	NULL},
{ ram_perc,		"  %s%% |",		NULL},
{ run_command,	"  %s |",		"amixer sget Master | awk -F \"[][]\" 'NF>1 { print $2 }'"},
{ run_command,	" %s |",		"amixer sget Capture | awk -F \"[][]\" '/Left:/ { print $2 }'"},
{ datetime,	"%s",	"%y-%m-%d %T %a" },
```
yaocccc

```
{ run_command,	"  %s |",		"amixer sget Master | awk -F \"[][]\" '/Left:/ { print $2 }'"},
```

### 音量调节

dwm 目录下，创建脚本。

```
sudo nvim voldown.sh

#!/bin/bash
amixer sset Master 5%- unmute

sudo nvim volup.sh

#!/bin/bash
amixer sset Master 5%+ unmute

sudo nvim voltoggle.sh

#!bash
amixer sset Master toggle
```

修改文件所属

```
sudo chown 用户名:用户名 voldown.sh
sudo chown 用户名:用户名 volup.sh
sudo chown 用户名:用户名 voltoggle.sh
```

赋予执行权限

```
chmod +x voldown.sh
chmod +x volup.sh
chmod +x voltoggle.sh
```

编辑 /dwm/config.h

```
#include <X11/XF86keysym.h>
static const char *voldown[] = { "/home/david/Downloads/dwm/voldown.sh", NULL }
static const char *volup[] = { "/home/david/Downloads/dwm/volup.sh", NULL }
static const char *voltoggle[] = { "/home/david/Downloads/dwm/voltoggle.sh", NULL }

{ MODKEY,	XK_F1,	   spawn,	SHCMD("amixer sset Master toggle") },
{ MODKEY,	XK_F2,	   spawn,	SHCMD("amixer sset Master 5%- ") },
{ MODKEY,	XK_F3,	   spawn,	SHCMD("amixer sset Master 5%+ ") },
```

### 笔记本屏幕亮度调节

intel 显卡可使用 xorg-xbacklight 包的 xbacklight 命令调节亮度。

```
xbacklight -set 50
xbacklight -inc 10
xbacklight -dec 10
```

dwm 目录下，创建脚本。

```
sudo pacman -S light

sudo nvim lightdown.sh

#!/bin/bash
light -U 10

sudo nvim lightup.sh

#!/bin/bash
light -A 10
```

修改文件所属

```
sudo chown 用户名:用户名 lightdown.sh
sudo chown 用户名:用户名 lightup.sh
```

赋予执行权限

```
chmod +x lightdown.sh
chmod +x lightup.sh
```

编辑 /dwm/config.h

```
static const char *lightdown[] = { "/home/david/Downloads/dwm/lightdown.sh", NULL }
static const char *lightup[] = { "/home/david/Downloads/dwm/lightup.sh", NULL }

{ MODKEY,	XK_F4,	   spawn,	SHCMD("xbacklight -dec 10") },
{ MODKEY,	XK_F5,	   spawn,	SHCMD("xbacklight -inc 10") },
```



