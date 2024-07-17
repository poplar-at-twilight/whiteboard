---
comments: true
tags:
  - Linux
  - 笔记
---

# 初次启动

### 设置主机名

```
sudo hostnamectl set-hostname --pretty "White-Poplar's Laptop"
```
```
sudo hostnamectl set-hostname --static c004-h1
```

## 删除 Packagekit 和 Discover

```
sudo zypper rm -u PackageKit discover6; sudo zypper al discover6 PackageKit
```

## 删除软件包与软件原

删除多余的 openH264 软件源和 ISO 软件源。默认应该有如下软件源：

```
poplar@c004-h1:~> zypper lr
软件源优先级已生效：                                                                                                                        (细节请参考 'zypper lr -P')
      90 (更高的优先级) :  1 个软件源
      99 (默认优先级)   :  4 个软件源

# | Alias         | Name                        | Enabled | GPG Check | Refresh
--+---------------+-----------------------------+---------+-----------+--------
1 | Google-Chrome | Google-Chrome               | 是      | (r ) 是   | 否
2 | packman       | packman                     | 是      | (r ) 是   | 是
3 | repo-debug    | openSUSE-Tumbleweed-Debug   | 否      | ----      | ----
4 | repo-non-oss  | openSUSE-Tumbleweed-Non-Oss | 是      | (r ) 是   | 是
5 | repo-oss      | openSUSE-Tumbleweed-Oss     | 是      | (r ) 是   | 是
6 | repo-source   | openSUSE-Tumbleweed-Source  | 否      | ----      | ----
7 | repo-update   | openSUSE-Tumbleweed-Update  | 是      | (r ) 是   | 是
```

更新系统：

```
sudo zypper ref; sudo zypper dup
```

补齐缺失的软件包：

```
sudo zypper inr
```

## 多媒体播放器

添加 Packman 源：

```
sudo zypper ar -cfp 90 https://mirrors.ustc.edu.cn/packman/suse/openSUSE_Tumbleweed/ packman
```

更新并安装多媒体播放器：

```
sudo zypper refresh
```
```
sudo zypper dist-upgrade --from packman --allow-vendor-change
```
```
sudo zypper in vlc ffmpeg-7 deadbeef
```

## 安装基本工具

- `keepassxc`
- `proxychains-ng`
- `git-core`

快捷命令：

```
sudo zypper in keepassxc proxychains-ng git-core
```

### chrome

添加软件源：

```
wget https://dl.google.com/linux/linux_signing_key.pub; sudo rpm --import linux_signing_key.pub
```
```
sudo zypper ar http://dl.google.com/linux/chrome/rpm/stable/x86_64 Google-Chrome
```

安装软件包：

```
sudo rpm -i google-chrome-stable_current_x86_64.rpm
```

## 更换语言

在 YaST Language 将首选语言修改为简体中文，同时选择英文作为第二语言。

然后将 KDE 设置中的区域和语言更改为中文。

等待更新完成，重启系统。

## AMD GPU

要使用 AMD GPU 运行程序，使用：

```
DRI_PRIME=1
```

```
poplar@c004-h1:~> DRI_PRIME=1 glxinfo | grep "OpenGL renderer"
OpenGL renderer string: AMD Radeon RX 7600M XT (radeonsi, navi33, LLVM 18.1.6, DRM 3.57, 6.9.5-1-default)
poplar@c004-h1:~> DRI_PRIME=0 glxinfo | grep "OpenGL renderer"
OpenGL renderer string: AMD Radeon 780M (radeonsi, gfx1103_r1, LLVM 18.1.6, DRM 3.57, 6.9.5-1-default)
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
export PATH=/home/poplar/.local/bin:/home/poplar/bin:/home/poplar/bin/command:/usr/local/bin:/usr/bin:/bin
#自定义 $PATH 路径

export EDITOR=nano
#将默认文本编辑器指定为 nano

alias sha256sum-dir="find . -type f -exec sha256sum {} \; > ../checksum.sha256; mv ../checksum.sha256 ."
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

alias yt-dlp="proxychains4 yt-dlp"
#为下载工具设置代理

alias clean="clear; exit"
#适用于 vscode 内置终端的退出命令

alias font-ref="fc-cache -fv"
#刷新字体缓存

alias pings="ping opentuna.cn -c 6; ping baidu.com -c 6; ping 1.1.1.1 -c 6"
#测试网络连通性

alias update="sudo zypper ref; sudo zypper lu; flatpakx update"
#刷新软件源并列出可用的更新

alias venv-setup="python3 -m venv .venv"
#设置容器

alias venv="source .venv/bin/activate"
#启动容器环境

alias steam-proxy="set-proxy; env STEAM_FORCE_DESKTOPUI_SCALING=1.5 steam"
#设置代理，并启动 steam（1440p）

alias javax="java -jar "-Dhttp.proxyHost=127.0.0.1" "-Dhttp.proxyPort=7890" "-Dhttps.proxyHost=127.0.0.1" "-Dhttps.proxyPort=7890""
#为 java 设置代理
```