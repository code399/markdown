## Arch linux 基础安装

---



### 制作 Arch Linux 安装 U 盘

- 下载 Arch Linux [镜像文件](https://archlinux.org/download)，使用 [Rufus](https://rufus.ie/zh/) 或 [ventoy](https://www.ventoy.net/cn/doc_start.html) 制作 Arch Linux 安装 U 盘。

  

### 使用 U 盘启动进入 live 环境

1. 插入 U 盘，开机并进入 BIOS，关闭 Secure Boot 选项，选择 U 盘 UEFI 启动项，保存设置并退出。

2. 出现引导菜单时选择 Arch Linux install medium 并回车，以 root 身份登录进入虚拟控制台。

3. 禁用 reflector 服务，该服务会自己更新 mirrorlist（软件包管理器 pacman 的软件源）。在特定情况下，它会误删某些有用的源信息。

   ```
   systemctl stop reflector.service
   ```

4. 若字体太小可调整字体。

   ```
   setfont ter-120b
   ```

   

### 连接无线网络

1. 进入 iwd 交互式提示符

    ```
    $ iwctl
    ```

2. 列出所有 WiFi 设备，找到网络设备名称。( wlan0 )

    ```
    [iwd]# device list
    ```

    如果设备或其相应的适配器已关闭，将其打开。

    ```
    [iwd]# device wlan0 set-property Powered on
    [iwd]# adapter wlan0 set-property Powered on
    ```

    若已知当前无线网络名（wifi-name），可直接转至第 5 步。

3. 扫描可用网络

    ```
    [iwd]# station wlan0 scan
    ```

4. 列出所有可用网络

    ```
    [iwd]# station wlan0 get-networks
    ```

5. 连接到一个网络，若需要，会提示输入密码。

    ```
    [iwd]# station wlan0 connect wifi-name
    ```

6. 退出

    ```
    [iwd]# exit
    ```

    如有问题，可查看内核是否加载了无线网卡驱动。
    ```
    lspci -k | grep Network
    ```

**提示：**

>- 在 iwctl 提示符中，可以通过 Tab 键自动补全命令和设备名称。
>- 可以在不进入交互式提示符的情况下，将所有命令当作命令行参数使用。例如：iwctl device wlan0 show。

**注意：**

>- iwd 会自动将网络密码存储在 /var/lib/iwd 目录中，以后就可以使用其自动连接记住的网络。
>- 要连接的 wifi-name 里带空格的网络，连接时需用双引号将网络名称括起来。
>- iwd 仅支持 8 到 63 位 ASCII 编码字符组成的 PSK 密码。如果没有满足要求，会出现下列错误信息：PMK generation failed. Ensure Crypto Engine is properly configured。



### 校准系统时间

- 开启网络时间同步
  
  ```
  timedatectl set-ntp true
  ```



### 创建硬盘分区

1. 检查硬盘设备 ( sda )

    ```
    fdisk -l
    ```

2. 为指定硬盘分区

    ```
    fdisk /dev/sda
    ```

3. 进入交互式模式开始分区
>- EFI 分区（sda1）= 1G（至少 300M，安装多个内核至少 1G）
>
>- 交换空间（sda2）= 10G（建议 >= 物理内存的60%）
>
>- 根分区（sda3）= 128G（建议 >= 128G）
>
>- home 分区（sda4）= 剩余空间
>
>（m:帮助  g:创建新的 GPT 分区表  n:创建新分区  w:保存退出）



### 格式化分区 & 挂载分区

1. 格式化分区
   
    ```
    mkfs.fat -F32 /dev/sda1
    mkswap /dev/sda2
    mkfs.ext4 /dev/sda3
    mkfs.ext4 /dev/sda4
    ```
2. 挂载分区
    ```
    mount /dev/sda3 /mnt
    
    mkdir /mnt/home
    mount /dev/sda4 /mnt/home
    
    mkdir /mnt/boot
    mount /dev/sda1 /mnt/boot
    
    swapon /dev/sda2
    ```

    查看挂载情况
    
    ```
    df -h
    ```
    
    查看 swap 分区挂载情况
    
    ```
    free -h
    ```



### 添加国内镜像源

文件 /etc/pacman.d/mirrorlist 定义了软件包会从哪个镜像下载。在列表中越靠前的镜像在下载软件包时拥有越高的优先权。
- 编辑 /etc/pacman.d/mirrorlist 
  
    ```
    vim /etc/pacman.d/mirrorlist
    ```
    
- 添加国内镜像源 ( 放到列表最前面 )

  ```
  Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
  Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
  Server = https://mirrors.aliyun.com/archlinux/$repo/os/$arch
  ```
  
  自动生成国内镜像源 ( 可选 )
  
  ```
  reflector --country China --age 24 --sort rate --protocol https --save /etc/pacman.d/mirrorlist
  ```



### 安装 Arch Linux

- 安装基础包
  
  ```
  pacstrap /mnt base base-devel linux-headers linux linux-firmware
  ```
- 安装其它必要的软件
  
  ```
  pacstrap /mnt dhcpcd networkmanager neovim sudo bash-completion
  ```



### 配置系统

 1. 生成 fstab 文件

    ```
    genfstab -U /mnt >> /mnt/etc/fstab
    ```

    复查确保没有错误 ( 可选 )

    ```
    cat /mnt/etc/fstab
    ```

2. 切换到新安装系统

    ```
    arch-chroot /mnt
    ```

3. 设置时区

    ```
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
    ```

4. 将系统时间同步到硬件时间

    ```
    hwclock --systohc
    ```

5. 编辑 /etc/locale.gen 移除 en_US.UTF-8 及 zh_CN.UTF-8 的注释符号

    ```
    nvim /etc/locale.gen
    ```

6. 生成 locale 信息

    ```
    locale-gen
    ```

7. 创建 /etc/locale.conf 文件并添加 LANG=en_US.UTF-8

    ```
    nvim /etc/locale.conf
    ```

    或

    ```
    echo 'LANG=en_US.UTF-8' > /etc/locale.conf
    ```

8. 创建 hostname 文件并添加主机名（当前机器在网络上的名称）

    ```
    nvim /etc/hostname
    ```

9. 编辑 /etc/hosts

    ```
    nvim /etc/hosts
    ```

    添加如下内容：

    ```
    127.0.0.1    localhost
    ::1          localhost
    127.0.1.1    主机名.localdomain    主机名
    ```

10. 设置 root 密码

    ```
    passwd
    ```



### 安装引导程序

1. 安装软件包

    ```
    pacman -S grub efibootmgr intel-ucode
    ```

2. 安装 GRUB 到 EFI 分区

    ```
    grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=ARCH
    ```

3. 编辑 `/etc/default/grub` 文件

    ```
    nvim /etc/default/grub
    ```

    改成

    ```
    GRUB_CMDLINE_LINUX_DEFAULT="loglevel=5 nowatchdog"
    ```

4. 生成 grub.cfg 配置文件

    ```
    grub-mkconfig -o /boot/grub/grub.cfg
    ```



### 重启

1. 退回安装环境
   
    ```
    exit
    ```
2. 卸载被挂载的分区
   
    ```
    umount -R /mnt
    ```
3. 拔掉 U 盘，重启。
   
    ```
    reboot
    ```



### 新系统基础配置

1. 以 root 账号登陆系统。

2. 设置终端字体

   - 下载字体

     ```
     pacman -S terminus-font
     ```

   - 临时设置字体

     ```
     setfont ter-120b
     ```

   - 永久设置（需要重启）

     ```
     nvim /etc/vconsole.conf
     ```

     添加

     ```
     FONT=ter-v24b
     ```

3. 链接无线网络

   ```
   systemctl enable --now NetworkManager
   nmcli dev wifi connect "wifi-name" password "wifi-password"
   ```

   附近 wifi 列表 ( 可选 )

   ```
   nmcli dev wifi list
   ```

4. 更新系统

   ```
   pacman -Syyu
   ```

5. 查看系统信息

   ```
   pacman -S neofetch terminus-font
   neofetch
   ```

6. 配置 root 账户的默认编辑器

   ```
   nvim ~/.bash_profile
   ```

   在适当位置加入以下内容

   ```
   export EDITOR='nvim'
   ```

7. 添加新用户

   添加用户

   ```
   useradd -m -G wheel -s /bin/bash 用户名
   ```

   设置用户密码

   ```
   passwd 用户名
   ```

   使用 nvim 编辑器通过 visudo 命令编辑 sudoers 文件

   ```
   EDITOR=nvim visudo
   ```

   找到如下一行，去掉注释符号。

   ```
   #%wheel ALL=(ALL:ALL) ALL
   ```

8. 开启 32 位支持库与 ArchLinux 中文社区仓库（archLinuxcn）

   编辑 /etc/pacman.conf 文件，去掉 \[multilib\] 一节中的注释，开启 32 位库支持。

   ```
   nvim /etc/pacman.conf
   ```

   添加 archlinuxcn 源（选择其一）

   ```
   [archlinuxcn]
   Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
   Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
   Server = https://mirrors.hit.edu.cn/archlinuxcn/$arch
   ```

   同步源

   ```
   pacman -Syu
   ```

   导入 GPG key

   ```
   pacman -S archlinuxcn-keyring
   ```

   更新系统

   ```
   pacman -Syyu
   ```

   重启

   ```
   reboot
   ```

   注： archlinuxcn 全部仓库[地址](https://github.com/archlinuxcn/mirrorlist-repo#arch-linux-cn-community-repo-mirrors-list)

至此，一个基础的、无图形界面的 ArchLinux 安装完成！



### 参考

- [官方安装指南](https://wiki.archlinuxcn.org/wiki/%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97)  

- [archlinux 简明指南](https://arch.icekylin.online/)  

- [Arch Linux 安装使用教程](https://archlinuxstudio.github.io/ArchLinuxTutorial/#/)