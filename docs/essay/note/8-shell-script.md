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
# 本脚本用于管理 openSUSE Tumbleweed ISO 文件。
#
# 主要功能有：
# 1. 下载 DVD/Live ISO 文件、sha256 文件和 asc 证书文件
# 2. 校验新旧 ISO 文件（计算哈希，验证 GPG 签名）
# 3. 更新替换旧版 ISO 文件
#
# 注意：
# 1. 使用前请完整阅读一遍脚本，使用时风险自负。
# 2. 用户需要自行修改需要修改的变量。
# 3. 用户需要自己配置 gpg 工具。

printf 'Please modify the script before using it for the first time to make it work properly.\n'

export ISO_DIR=$HOME/Downloads/ISO
# 默认保存位置
export ISO_DIR_TMEP=$HOME/Downloads/iso-m-temp
# 默认缓存位置

# 选择一个镜像站，一次只能选择一个
export DL_URL=https://mirrors.zju.edu.cn/opensuse
#export DL_URL=http://mirror.bjtu.edu.cn/opensuse
#export DL_URL=https://mirror.nyist.edu.cn/opensuse
#export DL_URL=https://ftp.sjtu.edu.cn/opensuse
#export DL_URL=https://mirrors.163.com/openSUSE
#export DL_URL=https://mirrors.bfsu.edu.cn/opensuse
#export DL_URL=https://mirrors.tuna.tsinghua.edu.cn/opensuse
#export DL_URL=https://mirrors.ustc.edu.cn/opensuse
#export DL_URL=
#export DL_URL=
# 默认包含已加入 mirrors.opensuse.org，位于中国的镜像站

DVD_NEW=$ISO_DIR_TMEP/openSUSE-Tumbleweed-DVD-x86_64-Current.iso
LIVE_NEW=$ISO_DIR_TMEP/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso
# 新文件检查位置
DVD_OLD=$ISO_DIR/openSUSE-Tumbleweed-DVD-x86_64-Current.iso
LIVE_OLD=$ISO_DIR/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso
# 老文件检查位置

while true; do

    printf -- '-%0.s' {1..100} && echo
    printf 'You can use the ISO files maintenance script for:\n\n'
    printf '1 - Download openSUSE DVD ISO files\n'
    printf '2 - Download openSUSE KDE Live ISO files\n'
    printf 'V - Verify New ISO files.\n'
    printf 'C - Check old ISO files.\n'
    printf 'U - Update ISO files\n'
    printf 'R - Clear temp file.\n'
    printf 'Q - End task.\n\n'
    printf '==> '
    read answer

    # 下载 ISO 文件
    if [ $answer = 1 ]; then
        if [ -d $ISO_DIR_TMEP ]; then
            printf 'INFO: Find the temp dir.\n'
            cd $ISO_DIR_TMEP
        else
            mkdir -p $ISO_DIR_TMEP
            printf 'INFO: The temp dir created.\n'
            cd $ISO_DIR_TMEP
        fi
        wget -c $DL_URL/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso
        wget -c $DL_URL/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso.sha256
        wget -c $DL_URL/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso.sha256.asc
    elif [ $answer = 2 ]; then
        if [ -d $ISO_DIR_TMEP ]; then
            printf 'INFO: Find the temp dir.\n'
            cd $ISO_DIR_TMEP
        else
            mkdir -p $ISO_DIR_TMEP
            printf 'INFO: The temp dir created.\n'
            cd $ISO_DIR_TMEP
        fi
        wget -c $DL_URL/tumbleweed/iso/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso
        wget -c $DL_URL/tumbleweed/iso/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso.sha256
        wget -c $DL_URL/tumbleweed/iso/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.iso.sha256.asc

    elif [ "$answer" = "V" ] || [ "$answer" = "v" ]; then
    # 校验新下载的 ISO 文件
        if [ -d $ISO_DIR_TMEP ]; then
            cd $ISO_DIR_TMEP
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
        else
            printf 'ERROR: No found the temp dir!\n'
        fi


    elif [ "$answer" = "C" ] || [ "$answer" = "c" ]; then
    # 校验旧版 ISO 文件
        if [ -d $ISO_DIR ]; then
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
        else
            printf 'ERROR: No found the ISO dir!\n'
        fi

    elif [ "$answer" = "U" ] || [ "$answer" = "u" ]; then
    #更新 ISO 文件
        if [ -d $ISO_DIR_TMEP ]; then
            if [ -f $DVD_NEW ]; then
                printf 'Found the new DVD ISO files: OK!\n'
                if [ -f $DVD_OLD ]; then
                    rm $ISO_DIR/openSUSE-Tumbleweed-DVD*.*
                    printf 'Delete the old DVD ISO files: OK!\n'
                else
                    printf 'ERROR: No found the old DVD ISO files!\n'
                fi
                mv $ISO_DIR_TMEP/openSUSE-Tumbleweed-DVD*.* $ISO_DIR
                printf 'Update the DVD ISO files: OK!\n'
            else
                printf 'ERROR: No found the new DVD ISO files!\n'
            fi
            if [ -f $LIVE_NEW ]; then
                printf 'Found the new Live ISO files: OK!\n'
                if [ -f $LIVE_OLD ]; then
                    rm $ISO_DIR_TMEP/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.*
                    printf 'Delete the old Live ISO files: OK!\n'
                else
                    printf 'ERROR: No found the old Live ISO files!\n'
                fi
                mv $ISO_DIR_TMEP/openSUSE-Tumbleweed-KDE-Live-x86_64-Current.* $ISO_DIR
                printf 'Update the Live ISO files: OK!\n'
            else
                printf 'ERROR: No found the new Live ISO files!\n'
            fi
        else
            printf 'ERROR: No found the temp dir!\n'
        fi

    elif [ "$answer" = "R" ] || [ "$answer" = "r" ]; then
    # 手动删除缓存文件夹
        if [ -d $ISO_DIR_TMEP ]; then
            rm -rf $ISO_DIR_TMEP
            printf 'Delete the temp dir: OK!\n'
        else
            printf 'ERROR: NO found the temp dir!'
        fi

    elif [ "$answer" = "Q" ] || [ "$answer" = "q" ]; then
        if [ -d "$ISO_DIR_TMEP" ]; then
            if [ "$(ls -A $ISO_DIR_TMEP)" ]; then
                printf "INFO: the temp dir isn't empty.\n"
            else
                rm -rf $ISO_DIR_TMEP
                printf "INFO: Deleted empty temp dir.\n"
            fi
        else
            printf 'INFO: No found the temp dir.\n'
        fi
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