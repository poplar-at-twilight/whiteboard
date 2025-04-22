---
tags:
  - Linux
  - 笔记
---

# Shell 脚本与配置文件

## bashrc

```shell
## 代理设置

alias pyx='https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890'
alias dnfx='pyx dnf'
# 对 dnf 设置代理
alias flatpakx="pyx flatpak --user"
# 对 flatpak 使用代理，并增加 --user 标签
alias set-proxy="export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890"
alias unset-proxy="unset http_proxy; unset https_proxy; unset all_proxy"
# 代理变量手动开关
alias steam-proxy="set-proxy; steam"
# 设置代理，并启动 steam
#alias steam-proxy="set-proxy; env STEAM_FORCE_DESKTOPUI_SCALING=1.5 steam"
# 设置代理，并启动 steam（150% 缩放）
#alias zypper='pyx ZYPP_PCK_PRELOAD=1 ZYPP_CURL2=1 zypper'
# 对 zypper 启用代理和并行下载
#alias zypper='ZYPP_PCK_PRELOAD=1 ZYPP_CURL2=1 zypper'
# 对 zypper 启用并行下载

## 软件源命令

#alias mirror-add="sudo zypper mr -da; sudo zypper ar -cfg 'https://mirror.nyist.edu.cn/opensuse/tumbleweed/repo/oss/' mirror-oss; sudo zypper ar -cfg 'https://mirror.nyist.edu.cn/opensuse/tumbleweed/repo/non-oss/' mirror-non-oss"
# 添加软件源
#alias packman-update="sudo zypper dup --from packman --allow-vendor-change"
#alias packman-add="sudo zypper ar -cfp 90 https://mirror.nyist.edu.cn/packman/suse/openSUSE_Tumbleweed/ packman"
# packman 相关的命令
alias flatpak-add="set-proxy; flatpak remote-add --if-not-exists --user flathub https://dl.flathub.org/repo/flathub.flatpakrepo"
# 添加 flathub

## shell 自定义

alias ls="ls --block-size=KiB --color=auto"
# 以 KiB 为单位显示文件大小
export PATH=/home/poplar/.local/bin:/home/poplar/bin:/home/poplar/bin/command:/usr/local/bin:/usr/bin:/bin
# 自定义 $PATH 路径
unset GTK_IM_MODULE
unset QT_IM_MODULE
# 针对 wayland 会话的 Fcitx5 环境变量
alias sudo="sudo "
# 对 sudo 后的字符启用别名
alias set-hostname="sudo hostnamectl set-hostname --pretty 'C004-H1' && sudo hostnamectl set-hostname --static Greysia"
# 设置主机名

## 实用命令

alias sha256sum-dir="find . -type f -exec sha256sum {} \; > ../checksum.sha256; mv ../checksum.sha256 .; echo 'Complete calculation!'"
# 自动计算当前文件夹内的全部文件的哈希，并将结果写入 sha256 文件
alias git-repo-clean="git remote prune origin && git repack && git prune-packed && git reflog expire --expire=1.month.ago && git gc --aggressive"
# 清理并压缩 git 仓库
alias pings="ping mirror.nyist.edu.cn -c 6; ping bing.com -c 6; ping 1.1.1.1 -c 6"
# 测试网络连通性
alias update="sudo zypper ref; sudo zypper lu; flatpakx update"
# 刷新软件源并列出可用的更新
alias venv-setup="python3 -m venv venv"
alias venv="source venv/bin/activate"
#启动 python 容器环境
```

## update-code

```shell
#!/bin/sh
#本脚本用于更新 VScodium

FILE=/home/poplar/Downloads/VSCodium*.tar.gz

if [ -f $FILE ]; then
    mv /home/poplar/bin/codium/data /home/poplar/bin/data
    printf 'Back up data: OK!\n'
    rm -r /home/poplar/bin/codium/*
    printf 'Remove old exc: OK!\n'
    tar -xf /home/poplar/Downloads/VSCodium*.tar.gz -C /home/poplar/bin/codium
    printf 'Software update: OK!\n'
    mv /home/poplar/bin/data /home/poplar/bin/codium/data
    printf 'Data restored: OK!\n'
    rm /home/poplar/Downloads/VSCodium*.tar.gz
    printf 'Clear tarball: OK!\n'

else
    printf 'ERROR: Update tarball no found!\n'
fi
```

----

## 配置文件

### git

在 `~/.gitconfig` 中，写入：

```
[user]
    name = Poplar at twilight
    email = poplar.cubic@gmail.com
[http]
    proxy = http://127.0.0.1:7890
```

### python

设置代理（`~/.config/pip/pip.conf`）：

```
[global]
proxy=http://localhost:7890
```