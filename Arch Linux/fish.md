## fish

---

```
sudo pacman -S fish
chsh -s /bin/fish
```

### 登录 fish 时自动起动 X

`~/.config/fish/config.fish`

```
# Start X at login
if status --is-interactive
    if test -z "$DISPLAY" -a $XDG_VTNR = 1
        exec startx -- -keeptty
    end
end
```

### 关闭问候语

```
set -U fish_greeting
```

