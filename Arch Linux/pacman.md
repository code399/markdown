

# 安装软件

\+ `pacman -S (软件名)`：安装软件，若有多个软件包，空格分隔

\+ `pacman -S --needed （软件名）`：安装软件，若存在，不重新安装最新的软件

\+ `pacman -Sy (软件名)`：安装软件前，先从远程仓库下载软件包数据库

\+ `[pacman] -Sv (软件名)`：输出操作信息后安装

\+ `[pacman -Sw] (软件名)`：只下载软件包，而不安装

\+ `pacman -U (软件名)`：安装本地软件包

\+ `pacman -U ([http://www.xxx.com/xxx.pkg.tar.xz])`：安装一个远程包

# 卸载软件

\+ `pacman -R (软件名)`：只卸载软件包不卸载依赖的软件

\+ `pacman -Rv (软件名)`：卸载软件，并输出卸载信息

\+ `pacman -Rs (软件名)`：卸载软件，并同时卸载该软件的依赖软件

\+ `pacman -Rsc (软件名)`：卸载[软件](https://www.zhihu.com/search?q=软件&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2444519204})，并卸载依赖该软件的程序

\+ `pacman -Ru (软件名)`：卸载软件，同时卸载不被任何软件所依赖

# 搜索软件

\+ `pacman -Ss (关键字)`：在仓库搜索包含关键字的软件包

\+ `pacman -Sl `：显示软件仓库所有软件的列表

\+ `pacman -Qs (关键字)`：搜索已安装的软件包

\+ `pacman -Qu`：列出可升级的软件包

\+ `pacman -Qt`：列出不被任何软件要求的软件包

\+ `pacman -Q (软件名)`：查看软件包是否已安装

\+ `pacman -Qi (软件包)`：查看某个软件包详细信息

\+ `pacman -Ql (软件名)`：列出软件包所有文件安装路径

# 软件包组

\+ `pacman -Sg`：列出软件仓库上所有软件包组

\+ `pacman -Qg`：列出本地已经安装的软件包组和子软件包

\+ `pacman -Sg (软件包组)`：查看软件包组所包含的软件包

\+ `pacman -Qg (软件包组)`：查看软件包组所包含的软件包

# 更新系统

\+ `pacman -Sy`：从服务器下载最新的软件包数据库到本地

\+ `pacman -Su`：升级所有已安装的软件包

\+ `pacman -Syu`：升级整个系统

# 清理缓存

\+ `pacman -Sc`：清理未安装的软件包文件

\+ `pacman -Scc`：清理所有的缓存文件