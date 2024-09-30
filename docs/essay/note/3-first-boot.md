---
tags:
  - Linux
  - 笔记
---

# 初次启动

## 变更所有权

第二块硬盘在初始化后，默认属于 root 用户组，执行下列命令更换所有权：

```
cd home; sudo chown poplar:poplar bt
```

## 设置主机名

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
sudo zypper in vlc ffmpeg-7
```

## 安装基本工具

- `keepassxc`
- `proxychains-ng`
- `git-core`

快捷命令：

```
sudo zypper in keepassxc proxychains-ng git-core
```

### chromium

添加软件源：

```
sudo zypper in chromium chromium-ffmpeg-extra chromium-plugin-widevinecdm
```

安装软件包：

```
sudo zypper in google-chrome-stable
```

## 更换语言（可选操作）

在 YaST Language 将首选语言修改为简体中文，同时选择英文作为第二语言。

然后将 KDE 设置中的区域和语言更改为中文。

等待更新完成，重启系统。

## AMD GPU

以核显运行应用的环境变量：

```
DRI_PRIME=0
```

以独显运行程序的环境变量：

```
DRI_PRIME=1
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

alias packman-update="sudo zypper dup --from packman --allow-vendor-change"
#从 packman 更新，并允许供应商切换
```