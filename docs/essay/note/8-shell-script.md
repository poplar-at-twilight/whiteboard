---
tags:
  - Linux
  - 笔记
---

# Shell 脚本与配置文件

shell 脚本一般放置在 `~/bin/command`

* 在其他文件中已提及的 shell 脚本不会在此处列出。

## aria2-m

```shell
#!/bin/sh
#本脚本用于维护 aria2 及 ISO 的下载管理

export ISO_DL_DIR=/home/poplar/Downloads/Aria2
export ISO_DIR=/home/poplar/Downloads/ISO
export ARIA2_DIR=/home/poplar/bin/aria2
#设定目录

printf 'You can use this script to verify ISO files, register Aria2 service and clean up aria2 logs.\n'

while true; do
    printf 'V - Verify ISO files\nR - Register Aria2 service\nC - Clean up aria2 logs\nQ - Quit script' && echo
    read answer

    if [ "$answer" = "V" ] || [ "$answer" = "v" ]; then
    #校验 ISO 文件
        cd $ISO_DL_DIR;ls -l && echo
        sha256sum -c openSUSE*.sha256
        gpg --verify openSUSE*.asc
        #验证完整性
        rm $ISO_DIR/openSUSE*.*
        mv $ISO_DL_DIR/openSUSE*.* $ISO_DIR
        #移动文件至默认位置
        echo "ISO files updated!"
        printf -- '-%0.s' {1..100} && echo
        #分隔符
    elif [ "$answer" = "R" ] || [ "$answer" = "r" ]; then
        echo "Registering systemd services for aria2..."
        sudo cp $ARIA2_DIR/aria2.service /etc/systemd/system
        echo "The service files have been copied to /etc."
        #拷贝 service 文件
        sudo systemctl daemon-reload
        sudo systemctl enable aria2 --now
        echo "Service started!"
        printf -- '-%0.s' {1..100} && echo
    elif [ "$answer" = "C" ] || [ "$answer" = "c" ]; then
        ls -lh $ARIA2_DIR/aria2.log
        #读取文件大小
        sudo systemctl status aria2 | grep "Active"
        #查询状态
        sudo systemctl stop aria2
        #关闭服务
        rm $ARIA2_DIR/aria2.log; touch $ARIA2_DIR/aria2.log
        echo "Logs cleaned"
        #清理日志文件
        sudo systemctl restart aria2
        #重启服务
        sudo systemctl status aria2 | grep "Active"
        ls -lh $ARIA2_DIR/aria2.log
        #查询状态
        printf -- '-%0.s' {1..100} && echo
    elif [ "$answer" = "Q" ] || [ "$answer" = "q" ]; then
        clear; exit
    else
    #重新开始循环
        echo "ERROR! Invalid input."
        continue
    fi
done
```

### update-code

```shell
#!/bin/sh
#本脚本用于更新 VScodium

ls -l ~/Downloads && echo
mv /home/poplar/bin/codium/data /home/poplar/bin/data
rm -r /home/poplar/bin/codium/*
echo 'Data backed up.'
tar -xf /home/poplar/Downloads/VSCodium*.tar.gz -C /home/poplar/bin/codium
echo 'Codium Updated.'
mv /home/poplar/bin/data /home/poplar/bin/codium/data
echo 'Data restored.'
rm /home/poplar/Downloads/VSCodium*.tar.gz
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

### bashrc

`~/.bashrc`：

```shell
alias ls="ls --block-size=KiB"
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
alias flatpak-clear="flatpak uninstall --unused"
#清理不需要的库

alias pings="ping mirror.sjtu.edu.cn -c 6; ping baidu.com -c 6; ping 1.1.1.1 -c 6"
#测试网络连通性

alias update="sudo zypper ref; sudo zypper lu; flatpakx update"
#刷新软件源并列出可用的更新

alias venv-setup="python3 -m venv venv"
#设置容器

alias venv="source venv/bin/activate"
#启动容器环境

alias steam-proxy="set-proxy; env STEAM_FORCE_DESKTOPUI_SCALING=1.5 steam"
#设置代理，并启动 steam（1440p）

alias update-packman="sudo zypper dup --from packman --allow-vendor-change"
alias add-packman="sudo zypper ar -cfp 90 https://mirrors.ustc.edu.cn/packman/suse/openSUSE_Tumbleweed/ packman"
#packman 相关的命令
```

### mangohud & Goverlay

对于 steam 游戏，使用 `mangohud %command%`。使用说明详见：`man mangohud`

注意：

1. 不要在游戏启动时让 mangohud 读取游戏的平均帧和 1% low 帧
1. 使用 Goverlay 时，如果有按钮被遮挡，可以把显示器缩放比例从 150% 调整为 100%

- `/home/poplar/.config/MangoHud/MangoHud.conf`：

```shell
################### File Generated by Goverlay ###################
legacy_layout=false
custom_text_center=mangohud

background_alpha=0.6
round_corners=0
background_alpha=0.6
background_color=000000

font_size=24
text_color=FFFFFF
position=top-left

pci_dev=0:03:00.0
table_columns=3
gpu_text=GPU
gpu_stats
gpu_load_change
gpu_load_value=50,90
gpu_load_color=FFFFFF,FFAA7F,CC0000
gpu_temp
gpu_mem_temp
gpu_power
gpu_color=2E9762
cpu_text=CPU
cpu_stats

cpu_load_change
cpu_load_value=50,90
cpu_load_color=FFFFFF,FFAA7F,CC0000
cpu_temp
cpu_color=2E97CB
swap
vram
vram_color=AD64C1
ram
ram_color=C26693
battery
battery_color=00FF00
fps
frame_timing
frametime_color=00FF00
fps_limit_method=late
toggle_fps_limit=Shift_L+F1

fps_limit=0
resolution
#offset=0





time#


output_folder=/home/poplar
log_duration=30
autostart_log=0
log_interval=100
toggle_logging=Shift_L+F2
```