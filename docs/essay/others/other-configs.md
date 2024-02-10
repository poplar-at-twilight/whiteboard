---
comments: true
tags:
  - 杂谈
  - Linux
  - openSUSE
---

# Tumbleweed 配置小记（二）

其他配置文件

## bashrc

在 `~/.bashrc` 中设置别名：

```shell
alias envycontrol="python3 /home/poplar/文档/git-repos/github/envycontrol/envycontrol.py"
#简化 envycontrol 命令

export EDITOR=nano
#将默认文本编辑器指定为 nano

alias sha256sum-dir="find . -type f -exec sha256sum {} \; > ../output.sha256; mv ../output.sha256 ."
#自动计算当前文件夹内的全部文件的哈希，并将结果写入 sha256 文件

alias git-repo-clean="git remote prune origin && git repack && git prune-packed && git reflog expire --expire=1.month.ago && git gc --aggressive"
#清理并压缩 git 仓库

alias set-proxy="export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890"
alias unset-proxy="unset http_proxy; unset https_proxy; unset all_proxy"
#设置环境变量

alias sudo="sudo "
#对 sudo 后的字符启用别名

alias pyc="proxychains4"
#更短的别名

#alias dnfx="proxychains4 dnf"
alias zypper="proxychains4 zypper"
#对 zypper/dnf 使用代理

alias flatpak="flatpak --user"
#增加 --user 标签

alias flatpakx="proxychains4 flatpak --user"
#对 flatpak 使用代理，并增加 --user 标签

alias yt-dlp="proxychains4 yt-dlp"
#为下载工具设置代理

alias nvidia="sudo prime-select nvidia"
alias intel="sudo prime-select intel"
alias gpu="sudo prime-select get-current"
#简化显卡切换命令

alias clean="clear; exit"
#适用于 vscode 内置终端的退出命令

alias font-ref="fc-cache -fv"
#刷新字体

alias pings="ping opentuna.cn -c 6; ping baidu.com -c 6; ping 1.1.1.1 -c 6"
#测试网络连通性

alias update="sudo zypper ref; sudo zypper lu; flatpakx update"
#刷新软件源并列出可用的更新

alias reboot="sudo reboot"
alias poweroff="sudo poweroff"
#为某些命令默认添加 sudo 权限

alias venv-setup="python3 -m venv .venv"
#设置容器
alias venv="source .venv/bin/activate"
#启动容器环境
```

## 配置 git

在 `~/.gitconfig` 中，写入：

```
[user]
	name = Poplar at twilight
	email = poplar.cubic@gmail.com
[http]
	proxy = http://127.0.0.1:7890
[safe]
	directory =/home/poplar/文档/git-repos/github/fzug.github.io
	directory =/home/poplar/文档/git-repos/github/page-opensuse-zh
	directory =/home/poplar/文档/git-repos/gitlab/whiteboard
	directory =/home/poplar/文档/git-repos/Reuleaux triangle
```

## Python

设置代理（`~/.config/pip/pip.conf`）：

```
[global]
proxy=http://localhost:7890
```

## TLP（可选）

```
cd /etc/tlp.d && ls
```

01-cpu.conf

```
CPU_SCALING_GOVERNOR_ON_AC=powersave
CPU_SCALING_GOVERNOR_ON_BAT=powersave

CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
CPU_ENERGY_PERF_POLICY_ON_BAT=power

CPU_BOOST_ON_AC=1
CPU_BOOST_ON_BAT=0

SCHED_POWERSAVE_ON_AC=0
SCHED_POWERSAVE_ON_BAT=1
```

02-usb.conf

```
USB_AUTOSUSPEND=1
```

03-battery.conf

```
START_CHARGE_THRESH_BAT0=50
# 开始充电阈值
STOP_CHARGE_THRESH_BAT0=80
# 停止充电阈值
```