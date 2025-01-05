---
tags:
  - Linux
  - 笔记
---

# Shell 脚本与配置文件

shell 脚本一般放置在 `~/bin/command`

* 在其他文件中已提及的 shell 脚本不会在此处列出。

### update-code

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

## iso-m

```shell
#!/bin/sh
#本脚本用于下载、校验和归档 openSUSE Tumbleweed ISO 文件

export ISO_DIR=/home/poplar/Downloads/ISO
export ISO_DIR_DL=/home/poplar/Downloads/Aria2
#文件目录

DVD_NEW=/home/poplar/Downloads/Aria2/openSUSE-Tumbleweed-DVD-x86_64-Current.iso
LIVE_NEW=/home/poplar/Downloads/Aria2/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso
DVD_OLD=/home/poplar/Downloads/ISO/openSUSE-Tumbleweed-DVD-x86_64-Current.iso
LIVE_OLD=/home/poplar/Downloads/ISO/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso
#要检测的 ISO 文件

while true; do

    printf -- '-%0.s' {1..100} && echo
    printf 'You can use the ISO files maintenance script for:\n\n'
    printf '1 - Download openSUSE DVD ISO files\n'
    printf '2 - Download openSUSE KDE Live ISO files\n'
    printf 'V - Verify New ISO files.\n'
    printf 'C - Check old ISO files.\n'
    printf 'U - Update ISO files\n'
    printf 'Q - End task.\n\n'
    printf '==> '
    read answer

    #下载 ISO 文件
    cd $ISO_DIR_DL
    if [ $answer = 1 ]; then
        wget https://mirrors.ustc.edu.cn/opensuse/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso
        wget https://mirrors.ustc.edu.cn/opensuse/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso.sha256
        wget https://mirrors.ustc.edu.cn/opensuse/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso.sha256.asc
    elif [ $answer = 2 ]; then
        wget https://mirrors.ustc.edu.cn/opensuse/tumbleweed/iso/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso
        wget https://mirrors.ustc.edu.cn/opensuse/tumbleweed/iso/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso.sha256
        wget https://mirrors.ustc.edu.cn/opensuse/tumbleweed/iso/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso.sha256.asc

    elif [ "$answer" = "V" ] || [ "$answer" = "v" ]; then
    #校验新下载的 ISO 文件
        cd $ISO_DIR_DL
        if [ -f $DVD_NEW ]; then
            gpg --verify openSUSE-Tumbleweed-DVD-x86_64-Current.iso.sha256.asc
            cat openSUSE-Tumbleweed-DVD-x86_64-Current.iso.sha256 | sed 's/openSUSE-Tumbleweed-DVD-x86_64-Snapshot\(........\)-Media.iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso/' > temp.sha256
            sha256sum -c temp.sha256
            rm temp.sha256
        else
            printf 'ERROR: No found new DVD ISO files!\n'
        fi
        printf -- '-%0.s' {1..100} && echo
        if [ -f $LIVE_NEW ]; then
            gpg --verify openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso.sha256.asc
            cat openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso.sha256 | sed 's/openSUSE-Tumbleweed-KDE-Live-x86_64-Snapshot\(........\)-Media.iso/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso/' > temp.sha256
            sha256sum -c temp.sha256
            rm temp.sha256
        else
            printf 'ERROR: No found new Live ISO files!\n'
        fi

    elif [ "$answer" = "C" ] || [ "$answer" = "c" ]; then
    #校验旧版 ISO 文件
        cd $ISO_DIR
        if [ -f $DVD_OLD ]; then
            gpg --verify openSUSE-Tumbleweed-DVD-x86_64-Current.iso.sha256.asc
            cat openSUSE-Tumbleweed-DVD-x86_64-Current.iso.sha256 | sed 's/openSUSE-Tumbleweed-DVD-x86_64-Snapshot\(........\)-Media.iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso/' > temp.sha256
            sha256sum -c temp.sha256
            rm temp.sha256
        else
            printf 'ERROR: No found old DVD ISO files!\n'
        fi
        printf -- '-%0.s' {1..100} && echo
        if [ -f $LIVE_OLD ]; then
            gpg --verify openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso.sha256.asc
            cat openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso.sha256 | sed 's/openSUSE-Tumbleweed-KDE-Live-x86_64-Snapshot\(........\)-Media.iso/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso/' > temp.sha256
            sha256sum -c temp.sha256
            rm temp.sha256
        else
            printf 'ERROR: No found old Live ISO files!\n'
        fi

    elif [ "$answer" = "U" ] || [ "$answer" = "u" ]; then
    #更新 ISO 文件
        if [ -f $DVD_NEW ]; then
            printf 'Found the new DVD ISO files: OK!\n'
            if [ -f $DVD_OLD ]; then
                rm $ISO_DIR/openSUSE-Tumbleweed-DVD*.*
                printf 'Delete the old DVD ISO files: OK!\n'
            else
                printf 'ERROR: No found the old DVD ISO files!\n'
            fi
            mv $ISO_DIR_DL/openSUSE-Tumbleweed-DVD*.* $ISO_DIR
            printf 'Update the DVD ISO files: OK!\n'
        else
            printf 'ERROR: No found the new DVD ISO files!\n'
        fi
        if [ -f $LIVE_NEW ]; then
            printf 'Found the new Live ISO files: OK!\n'
            if [ -f $LIVE_OLD ]; then
                rm $ISO_DIR/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.*
                printf 'Delete the old Live ISO files: OK!\n'
            else
                printf 'ERROR: No found the old Live ISO files!\n'
            fi
            mv $ISO_DIR_DL/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.* $ISO_DIR
            printf 'Update the Live ISO files: OK!\n'
        else
            printf 'ERROR: No found the new Live ISO files!\n'
        fi

    elif [ "$answer" = "Q" ] || [ "$answer" = "q" ]; then
        exit

    else
        printf 'ERROR: Invalid input!\n'
        continue

    fi
done
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

### proxychains-ng

`/etc/proxychains.conf`：

```
quiet_mode
http 127.0.0.1 7890
```

### python

设置代理（`~/.config/pip/pip.conf`）：

```
[global]
proxy=http://localhost:7890
```

### nano

如果 nano 编辑器没有语法高亮，则创建如下文件：

- `~/.nanorc`

```shell
include /usr/share/nano/*.nanorc
```

### bashrc

`~/.bashrc`：

```shell
alias ls="ls --block-size=KiB --color=auto"
#以 KiB 为单位显示文件大小

unset GTK_IM_MODULE
unset QT_IM_MODULE
#针对 wayland 会话的 Fcitx5 环境变量

export PATH=/home/poplar/.local/bin:/home/poplar/bin:/home/poplar/bin/command:/usr/local/bin:/usr/bin:/bin
#自定义 $PATH 路径

export EDITOR=nano
#将默认文本编辑器指定为 nano

alias sha256sum-dir="find . -type f -exec sha256sum {} \; > ../checksum.sha256; mv ../checksum.sha256 .; echo 'Complete calculation!'"
#自动计算当前文件夹内的全部文件的哈希，并将结果写入 sha256 文件

alias git-repo-clean="git remote prune origin && git repack && git prune-packed && git reflog expire --expire=1.month.ago && git gc --aggressive"
#清理并压缩 git 仓库

alias set-proxy="export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890"
alias unset-proxy="unset http_proxy; unset https_proxy; unset all_proxy"
#环境变量开关

alias sudo="sudo "
#对 sudo 后的字符启用别名

alias pyc="proxychains4"
#更短的别名

#alias dnfx="proxychains4 dnf"
#alias zypper="proxychains4 zypper"
#对 zypper/dnf 使用代理

alias flatpakx="proxychains4 flatpak --user"
#对 flatpak 使用代理，并增加 --user 标签

alias pings="ping mirror.sjtu.edu.cn -c 6; ping bing.com -c 6; ping 1.1.1.1 -c 6"
#测试网络连通性

alias update="sudo zypper ref; sudo zypper lu; flatpakx update"
#刷新软件源并列出可用的更新

alias venv-setup="python3 -m venv venv"
alias venv="source venv/bin/activate"
#启动 python 容器环境

alias steam-proxy="set-proxy; env STEAM_FORCE_DESKTOPUI_SCALING=1.5 steam"
#设置代理，并启动 steam（1440p）

alias packman-update="sudo zypper dup --from packman --allow-vendor-change"
alias packman-add="sudo zypper ar -cfp 90 https://mirrors.ustc.edu.cn/packman/suse/openSUSE_Tumbleweed/ packman"
#packman 相关的命令

alias apu-top="amdgpu_top --dark --apu"
alias gpu-top="amdgpu_top --dark --single"
#amdgpu_top 快捷命令
```