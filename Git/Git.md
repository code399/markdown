## Git

---

### 安装 Git

- Windows

  官网[下载](https://git-scm.com/downloads)

- Arch Linux

  ```
  sudo pacman -S git openssh
  ```

### 配置 Git

- 使用 Lunix 终端或 Windows Git Bash

  ```
  git config --global user.name "用户名"
  git config --global user.email "邮件地址"
  ```


### 本地仓库

- 创建本地仓库

  创建一个目录，并在此目录下执行命令：

  ```
  git init
  ```

  当前目录下会生成一个 .git 目录。将需要版本管理的文件拷贝到当前目录。

- 添加文件到仓库

  ```
  git add 文件名
  ```

- 提交文件到仓库

  ```
  git commit -m "提交说明"
  ```

  注：可提交多个文件后统一提交。

### 远程仓库

- 创建 SSH Key

  ```
  ssh-keygen -t rsa -C "邮件地址"
  ```

  用户主目录下 .ssh 目录里会生成两个文件（id_rsa`和`id_rsa.pub）

- 创建远程仓库
  
  - 注册并登录 GitHub -> 右上角 "+" -> New repostory -> 填入库名与说明 -> Create repository

  - 注册并登录 Gitee -> 右上角 "+" -> 新建仓库 -> 填入库名与说明 -> 创建
  
- 添加 SSH Key
  - GitHub -> 右上角头像 -> settings -> SSH and GPG keys -> New SSH Key -> 粘贴 id_rsa.pub 文件内容 -> Add SSH key
  - Gitee -> 右上角头像 -> 账号设置 -> SSH公钥 -> 填写一个便于识别的标题 -> 粘贴 id_rsa.pub 文件内容 -> 确定

  注：linux 环境下为方便复制粘贴，可安装 leafpad 编辑器。
  
- 关联远程仓库并首次推送

  - GitHub

    ```
    git remote add github git@github.com:用户名/仓库名.git
    git push -u github master
    ```

  - Gitee

    ```
  git remote add gitee git@gitee.com:用户名/仓库名.git
    git push -u gitee master
    ```
  
  - 以后推送命令

    ```
    git push github master
    git push gitee master
  
  

### 管理仓库

- 查看提交日志

  ```
  git log
  git log --pretty=oneline
  ```

- 查看命令日志

  ```
  git reflog
  ```

- 版本回退

  ```
  git reset --hard HEAD^
  git reset --hard 版本号前几位
  ```

- 删除文件

  ```
  git rm 文件名
  git commit -m "提交说明"
  ```

- 查看仓库当前状态

  ```
  git status
  ```

- 查看文件有何变动

  ```
  git diff readme.txt
  ```

  

