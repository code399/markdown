## 安装窗口管理器 DWM

---

### 安装基础组件

- net-tools：包含各种网络工具的库，例如  ifconfig、netstat 等。官方目前使用`ip address` 命令来获取本机的IP地址。

- man-db：提供 man 命令。

- man-pages：提供 man 页面内容。

- man-pages-zh_cn：简体中文版。

- texinfo：info 帮助文档的包。

- ntfs-3g：对NTFS文件系統提供支持。

- tree：以树形结构显示目录中各种文件的依附关系。

- pacman-contrib：使用 pactree 以目录树的形式显示依赖包的名称。

- wget：一个用來下载的工具

- git：^_^

- usbutils：查看系统USB设备

- pciutils：查看系统PCI设备

- acpi：用來查看电池电量的工具

```
sudo pacman -S net-tools man-db man-pages man-pages-zh_cn texinfo ntfs-3g tree pacman-contrib wget git usbutils pciutils acpi
```

### 安装字体

```
sudo pacman -S adobe-source-han-serif-cn-fonts wqy-zenhei wqy-microhei
sudo pacman -S noto-fonts-cjk noto-fonts-emoji noto-fonts-extra
sudo pacman -S nerd-fonts-bitstream-vera-sans-mono
sudo pacman -S nerd-fonts-dejavu-sans-mono
sudo pacman -S nerd-fonts-jetbrains-mono ttf-nerd-fonts-symbols
```

### 安装驱动

- 显卡驱动

```
sudo pacman -S xf86-video-intel mesa lib32-mesa vulkan-intel lib32-vulkan-intel
```

- 声卡与笔记本触控板驱动

```
sudo pacman -S alsa-utils xf86-input-libinput xf86-input-synaptics
```

- xorg、xorg-xinit：x 窗口相关。
- nigrogen：设置背景图片。
- feh
- udisks2、udiskie
- pcmanfm
- picom：窗口渲染，实现透明效果。

```
sudo pacman -S xorg xorg-xinit nitrogen picom
```

### 安装 DWM

```
git clone https://git.suckless.org/dwm --depth=1
git clone https://git.suckless.org/st --depth=1
git clone https://git.suckless.org/dmenu --depth=1

rm -rf fossproject/.git
```

分别切换到这几个下载下来的目录中，依次执行 `sudo make clean install` 进行编译安装

```
cp /etc/X11/xinit/xinitrc ~/.xinitrc
sudo nvim ~/.xinitrc
```

在文件中添加一行

```
exec dwm
```

注意：至少要确保 `/etc/X11/xinit/xinitrc` 中的最后一个 `if` 块出现在您的 `~/.xinitrc` 文件中。

`startx`：启动 DWM

`Shift + Alt + q`：退出 DWM

`Shift + Alt + Enter`：启动终端

`Shift + Alt + c`：关闭当前窗口

dwm中最重要的键是 `Mod1` 键，这个键默认映射到了 `Alt` 键，使用 `Mod1 + p` 可以启动 `dmenu`, 然后只需要在上边出现的工具条中输入你想运行的程序的前几个字母，也可以按左右箭头在进行选择，按回车键完成,即可启动想要的程序

可以使用 Shift + Mod1 + x来将当前的活动窗口移到其他的标签页，其中x是标签页的编号



