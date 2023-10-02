# 安装 Debian

---

1．[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/)
2．[中科大镜像站](https://mirrors.ustc.edu.cn/)
3．[阿里巴巴开源镜像站](https://developer.aliyun.com/mirror/)
4．[上海交通大学源镜像服务](https://mirror.sjtu.edu.cn/)

**常用分区方式**  
引导分区 挂载点/boot 分区格式ext4 1G以内即可  
交换分区 无挂载点 分区格式选择交换分区（swap） 最大不建议超过真实内存大小，除非内存小于2G，你的内存大于4G，推荐2G即可，没必要有些人推荐的与内存相当。  
主目录　挂载点/　分区格式ext4 大小大约20-40G  
家目录　挂载点/home 分区格式ext4 剩下的所有空间

**1. KDE**  
KDE 是 K Desktop Environment 的缩写，中文译为“K桌面环境”。KDE 是基于大名鼎鼎的 Qt的，最初于 1996 年作为开源项目公布，并在 1998 年发布了第一个版本，现在 KDE 几乎是排名第一的桌面环境了。许多流行的 Linux 发行版都提供了 KDE 桌面环境，比如 Ubuntu、Linux Mint、OpenSUSE、Fedora、Kubuntu、PC Linux OS 等。DE 和 Windows 比较类似，各位初学者相信都是 Windows 的用户，所以切换到 KDE 也不会有太大的障碍。  
**2. GNOME**  
GNOME 是 the GNU Network Object Model Environment 的缩写，中文译为“GNU网络对象模型环境”。GNOME 于 1999 年首次发布，现已成为许多Linux发行版默认的桌面环境（不过用得最多的是 Red Hat Linux）。GNOME 的特点是简洁、运行速度快，但是没有太多的定制选项，用户需要安装第三方工具来实现。  
**3. Unity**  
Unity 是由 Ubuntu 的母公司 Canonical 开发的一款外壳。之所以说它是外壳，是因为 Unity 运行在 GNOME 桌面环境之上，使用了所有 GNOME 的核心应用程序。  
**4. MATE**  
上面我们提到，GNOME 3 进行了全新的界面设计，这招致一些用户的不满，他们推出了其它的桌面环境，MATE 就是其中之一。  
**5. Cinnamon**  
与 MATE 类似，Cinnamon 是由 Linux Mint 团队因为不满 Gnome 3 的改进而开发的另一种桌面环境。但 Cinnamon 与 MATE 不同之处在于，Cinnamon 建立在 Gnome 3 的基础上。Cinnamon 是新的，而且在积极开发之中，但这款出色的桌面环境没有因新颖而在功能方面有所减弱。  
**6. Xfce**  
Xfce 是类 UNIX 操作系统上的轻量级桌面环境。虽然它致力于快速与低资源消耗，但仍然具有视觉吸引力且易于使用。

## 添加用户至 sudo 组

```
su
/usr/sbin/usermod -aG sudo 用户名
newgrp sudo
```

查看用户属于哪个组

```
groups 用户名
```

## 添加环境变量

```
sudo nano /etc/profile
export PATH=$PATH:/usr/sbin
source /etc/profile
```

## 禁用DVD/ISO CD-ROM软件包源

```
sudo nano /etc/apt/sources.list
```

## 设置中文输入法快捷键

设置 -> 键盘 -> 键盘快捷键 -> 打字 -> 切换至下个输入源 -> Ctrl+空格

## 设置启动终端快捷键

设置 -> 键盘 -> 键盘快捷键 -> 自定义快捷键
名称：open-terminal
全屏命令：gnome-terminal --full-screen
最大化命令：gnome-terminal --maximize
快捷键：Alt+Enter

## 设置触控板

轻击模拟鼠标点击 ( 默认为false )

```
gsettings set org.gnome.desktop.peripherals.touchpad tap-to-click true
```

调整触摸板速度 ( 默认为0 )

```
gsettings set org.gnome.desktop.peripherals.touchpad speed 0.57
```

打字时禁用触摸板 ( 默认为true )

```
gsettings set org.gnome.desktop.peripherals.touchpad disable-while-typing false
```

常用手势

- 单指：移动鼠标
- 双指上下：翻页
- 三指左右：切换Workspace
- 三指上：打开Overview (不常用, 按Super更快)
- 三指下：显示任务栏 (当你隐藏任务栏时)

## 窗口与工作区

工作区设置为静态

```
gsettings set org.gnome.mutter dynamic-workspaces false
```

工作区设置为10个

```
gsettings set org.gnome.desktop.wm.preferences num-workspaces 10
```

## 插件

[GNOME Shell extensions](https://extensions.gnome.org/)

[Dash to Dock](https://extensions.gnome.org/extension/307/dash-to-dock/)
Dock

[Dash to Panel](https://extensions.gnome.org/extension/1160/dash-to-panel/)

[transparent-top-bar(adjustable-transparency)](https://extensions.gnome.org/extension/3960/transparent-top-bar-adjustable-transparency/)  
让顶栏变透明的插件, 当窗口最大化或者与顶栏重叠时, 为了显示清晰会自动重新变回不透明

[color-app-menu-icon](https://extensions.gnome.org/extension/5473/color-app-menu-icon-for-gnome-40/)  
顶栏左上角会显示你当前所在应用的彩色图标与名称

[removable-drive-menu](https://extensions.gnome.org/extension/7/removable-drive-menu/)  
U 盘挂载托盘图标

[Vitals](https://extensions.gnome.org/extension/1460/vitals/)
系统监控

[color-pciker](https://extensions.gnome.org/extension/3396/color-picker/)  
拾色笔, 用来采集颜色。

[bluetooth-battery-indicator](https://extensions.gnome.org/extension/3991/bluetooth-battery/)  
当你连接蓝牙设备之后, 会在顶栏显示电量 

[bluetooth-quick-connect](https://extensions.gnome.org/extension/1401/bluetooth-quick-connect/)  
在 quick-setting 中, 让你快速连接/断开已经配对过的蓝牙设备, 非常有用

[space-bar](https://extensions.gnome.org/extension/5090/space-bar/)  
模仿 I3/Sway/Bspwm 等窗口管理器, 在左上角显示工作区, 有些类似的插件, 但个人认为, 这个插件最好

## snap

```
sudo apt update
sudo apt install snapd

reboot

sudo snap install core
```

## fish

```
sudo apt-get install fish
chsh -s /usr/bin/fish

sudo apt-get install git

git clone https://github.com/oh-my-fish/oh-my-fish
cd oh-my-fish
bin/install --offline

reboot

omf install pure
```
## V2RayA

安装 V2Ray 内核

```
sudo apt install curl
curl -Ls https://mirrors.v2raya.org/go.sh | sudo bash
```

关闭 V2Ray 服务

```
sudo systemctl disable v2ray --now
```

添加公钥

```
wget -qO - https://apt.v2raya.org/key/public-key.asc | sudo tee /etc/apt/keyrings/v2raya.asc
```

添加 V2RayA 软件源

```
echo "deb https://apt.v2raya.mzz.pub/ v2raya main" | sudo tee /etc/apt/sources.list.d/v2raya.list
sudo apt update
```

安装 V2RayA

```
sudo apt install v2raya
```

启动 v2rayA

```
sudo systemctl start v2raya.service
```

设置开机自动启动

```
sudo systemctl enable v2raya.service
```

## Neovim

snap 安装

```
sudo snap install nvim --classic
```

下载最新 Neovim 压缩包安装

```
sudo wget https://github.com/neovim/neovim/releases/download/stable/nvim-linux64.tar.gz
```

解压并移动

```
sudo tar xzvf nvim-linux64.tar.gz
sudo mv nvim-linux64 /usr/local/nvim
```

创建软链接

```
sudo ln -s /usr/local/nvim/bin/nvim /usr/bin/nvim
```

gcc

```
sudo apt install build-essential
sudo apt install manpages-dev
```

安装 Nerd 字体

下载字体压缩包

```
unzip JetBrainsMono.zip
sudo mv JetBrainsMono*.ttf /usr/share/fonts/
```

vscode

下载压缩包
```
su
dpkg -i filename.deb
```
