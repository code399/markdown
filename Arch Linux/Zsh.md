## Zsh

---

### Install & Configure

```
sudo pacman -S zsh zsh-completions
sudo pacman -S zsh-autosuggestions zsh-syntax-highlighting zsh-theme-powerlevel10k

chsh -s /usr/bin/zsh
```

首次进入 zsh 会进入配置菜单。( 1 -> 0 )

`~/.zshrc`

```
source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme

set nonomatch

alias ls='ls --color=auto'
alias grep='grep --color=auto'
```

powerlevel10k 主题在首次进入时，会触发配置界面。

### zsh & bash Hotkey

CTRL + A： 移动到行(line)的开始位置
CTRL + E: 移动到行的最后面
CTRL + [left arrow]: 向后移动一个单词(one word)
CTRL + [right arrow]: 向前移动一个单词
CTRL + T： 打开新的Tab标签
CTRL + U (BASH): 清除当前行光标位置后面的字符
CTRL + U (zsh): 如果是zsh，则将整行清除
ESC + [backspack]: 删除光标前面的单词
CTRL + W: 同上
ALT + D: 删除光标后面的单词
CTRL + R: 搜索历史
CTRL + G: 退出搜索模式
CTRL + _: 撤销最后一次的改变
CTRL + L: 清空屏幕
CTRL + S: 停止向屏幕继续输出
CTRL + Q: 重新开启屏幕输出
CTRL + C: 终止或者杀死当前前景进程(foreground process)
CTRL + Z: 暂停或者停止当前前景进程
!!: 执行历史记录中的上一个命令
!abc: 执行历史记录中的以 abc开头的命令
!abc:p: 打印历史记录中以abc开头的命令