### Intel 核显
[ArchWiki 文档](https://wiki.archlinuxcn.org/wiki/Intel_%E5%9B%BE%E5%BD%A2%E5%A4%84%E7%90%86%E5%99%A8)

```
sudo pacman -S xf86-video-intel mesa lib32-mesa vulkan-intel lib32-vulkan-intel
```

```
sudo pacman -S xf86-video-intel mesa-amber lib32-mesa-amber vulkan-intel lib32-vulkan-intel
```







---

Intel核心显卡

    sudo pacman -S xf86-video-intel lib32-mesa vulkan-intel lib32-vulkan-intel

Intel显卡驱动导致部分录屏软件花屏的解决办法：

    sudo vim /etc/X11/xorg.conf.d/00-modesetting.conf

添加以下内容：

Section "Device"
   Identifier "Intel"
   Driver "modesetting"
EndSection
