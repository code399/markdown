# Arch linux 基础安装

#David  #科学技术/计算机/操作系统/Linux/Arch 

---

### 制作 Arch Linux 安装 U 盘

下载 Arch Linux [镜像文件](https://archlinux.org/download)，使用 [Rufus](https://rufus.ie/zh/) 或 [ventoy](https://www.ventoy.net/cn/doc_start.html) 制作 Arch Linux 安装 U 盘。

### 使用 U 盘启动进入 live 环境

1．插入 U 盘，开机并进入 BIOS，关闭 Secure Boot 选项，选择 U 盘 UEFI 启动项，保存设置并退出。

2．出现引导菜单时选择 Arch Linux install medium 并回车，以 root 身份登录进入虚拟控制台。

3．调整终端字体。禁用 reflector 服务，该服务会自己更新 mirrorlist（软件包管理器 pacman 的软件源）。在特定情况下，它会误删某些有用的源信息。

```shell
setfont ter-120b
systemctl stop reflector.service
```

### 连接无线网络

1．进入 iwd 交互式提示符

```bash
iwctl
```

2．列出所有 WiFi 设备，找到网络设备名称。( 本例网络设备名称为 wlan0 )

```
[iwd]# device list
```

如果设备或其相应的适配器已关闭，将其打开。

```
[iwd]# device wlan0 set-property Powered on
[iwd]# adapter wlan0 set-property Powered on
```

若已知当前无线网络名（wifi-name），可直接转至第 5 步。

3．扫描可用网络
```
[iwd]# station wlan0 scan
```

4．列出所有可用网络

```
[iwd]# station wlan0 get-networks
```

5．连接到一个网络，若需要，会提示输入密码。

```
[iwd]# station wlan0 connect wifi-name
```

6．退出

```
[iwd]# exit
```

如有问题，可查看内核是否加载了无线网卡驱动。

```shell
lspci -k | grep Network
```

```
$ iwctl --passphrase wifi-passwd station wlan0 connect wifi-name
```
### 校准系统时间

```shell
timedatectl set-ntp true
```

### 创建硬盘分区

```shell
fdisk -l       # 检查硬盘设备 (本例硬盘设备为 sda)
fdisk /dev/sda # 为指定硬盘分区
```

进入交互式模式开始分区

| 分区              | 容量     | 说明                           |
| ----------------- | -------- | ------------------------------ |
| sda1 ( EFI )      | 1G       | 至少 300M，安装多个内核至少 1G |
| sda2 ( 交换空间 ) | 10G      | 建议 >= 物理内存的60%          |
| sda3 ( 根 )       | 128G     | 建议 >= 128G                   |
| sda4 ( home )     | 剩余空间 |                                |

m:帮助  g:创建新的 GPT 分区表  n:创建新分区  w:保存退出

### 格式化分区 & 挂载分区

1．格式化分区

```shell
# 格式化分区
mkfs.fat -F32 /dev/sda1
mkswap /dev/sda2
mkfs.ext4 /dev/sda3
mkfs.ext4 /dev/sda4

# 挂载分区
mount /dev/sda3 /mnt

mkdir /mnt/home
mount /dev/sda4 /mnt/home

mkdir /mnt/boot
mount /dev/sda1 /mnt/boot

swapon /dev/sda2

df -h   # 查看挂载情况（可选）
free -h # 查看 swap 分区挂载情况（可选）
```

### 添加国内镜像源

文件 /etc/pacman.d/mirrorlist 定义了软件包会从哪个镜像下载。在列表中越靠前的镜像在下载软件包时拥有越高的优先权。

```shell
vim /etc/pacman.d/mirrorlist # 添加如下国内镜像源 (放到列表最前面)

Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.aliyun.com/archlinux/$repo/os/$arch
```

### 安装 Arch Linux

```shell
pacstrap /mnt base base-devel linux-headers linux linux-firmware # 安装基础包
pacstrap /mnt dhcpcd networkmanager neovim sudo neofetch         # 安装其它必要的软件
```

### 配置系统

```shell
genfstab -U /mnt >> /mnt/etc/fstab                      # 生成 fstab 文件
cat /mnt/etc/fstab                                      # 复查确保没有错误 ( 可选 )

arch-chroot /mnt                                        # 切换到新安装系统

ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime # 设置时区
hwclock --systohc                                       # 将系统时间同步到硬件时间

nvim /etc/locale.gen                                    # 移除 en_US.UTF-8 及
                                                        # zh_CN.UTF-8 的注释符号
locale-gen                                              # 生成 locale 信息
echo 'LANG=en_US.UTF-8' > /etc/locale.conf

nvim /etc/hostname                                      # 添加主机名（当前机器在网络上的名称）
nvim /etc/hosts                                         # 添加如下内容

127.0.0.1    localhost
::1          localhost
127.0.1.1    主机名.localdomain    主机名

passwd                                                  # 设置 root 密码
```

### 安装引导程序

```shell
pacman -S grub efibootmgr intel-ucode                                       # 安装软件包
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=ARCH # 安装 GRUB

nvim /etc/default/grub # 改成 GRUB_CMDLINE_LINUX_DEFAULT="loglevel=5 nowatchdog"

grub-mkconfig -o /boot/grub/grub.cfg                                        # 生成配置文件
```

### 重启

```shell
exit           # 退回安装环境

umount -R /mnt # 卸载被挂载的分区

reboot         # 拔掉 U 盘，重启。
```

### 新系统基础配置

```shell
# 使用 root 账号登录系统

pacman -S terminus-font # 下载字体
setfont ter-120b        # 临时设置字体
nvim /etc/vconsole.conf # 添加 FONT=ter-v24b 永久设置字体 (需要重启)

systemctl enable --now NetworkManager                       # 开启网络管理器
nmcli dev wifi list                                         # 查看附近 wifi 列表 (可选)
nmcli dev wifi connect "wifi-name" password "wifi-password" # 连接无线网络

pacman -Syyu

useradd -m -G wheel -s /bin/bash 用户名 # 添加用户
passwd 用户名                           # 设置用户密码
EDITOR=nvim visudo                     # 去掉 #%wheel ALL=(ALL:ALL) ALL 的注释符号 (用户组权限)

nvim /etc/pacman.conf # 去掉 [multilib] 一节中的注释 (开启 32 位库支持)
                      # 添加如下 archlinuxcn 源 (选择其一) 开启 ArchLinux 中文社区仓库
[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
Server = https://mirrors.hit.edu.cn/archlinuxcn/$arch

pacman -Syu                   # 同步源
pacman -S archlinuxcn-keyring # 导入 GPG key

pacman -Syyu # 更新系统
reboot       # 重启
```

注： archlinuxcn 全部仓库[地址](https://github.com/archlinuxcn/mirrorlist-repo#arch-linux-cn-community-repo-mirrors-list)

至此，一个基础的、无图形界面的 ArchLinux 安装完成！

### 参考

- [官方安装指南](https://wiki.archlinuxcn.org/wiki/%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97) 

- [archlinux 简明指南](https://arch.icekylin.online/) 

- [Arch Linux 安装使用教程](https://archlinuxstudio.github.io/ArchLinuxTutorial/#/)