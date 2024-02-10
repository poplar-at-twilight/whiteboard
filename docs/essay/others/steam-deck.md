---
comments: true
tags:
  - 杂谈
  - Steam Deck
  - 网络
---

# Steam Deck 备忘录

!!! note "文档"
    更多内容详见：[archlinux 简明指南](https://arch.icekylin.online/) 和 [ArchLinux Wiki](https://wiki.archlinux.org/)。

## 设置环境变量

首先，启动 [Clash For windows](https://github.com/Fndroid/clash_for_windows_pkg/releases)。检查面板中的端口（`7890`）是否正常。如果不正常，应该需要重启程序。

!!! note
    - 推荐以便携模式启动 cfw。  
    - 要启动便携模式，请在 `cfw` 可执行文件所在的文件夹中，新建一个名为 `data` 文件夹。
    - 更多内容请阅读：[CFW documentation](https://docs.cfw.lbyczf.com/)  
    - TUN 模式需要额外的配置，详见[此处](https://docs.cfw.lbyczf.com/contents/tun.html#linux)。

打开终端模拟器 Konsole，输入以下内容：

```shell
export http_proxy=http://127.0.0.1:7890; export https_proxy=$http_proxy;
```

测试是否连接成功：

```shell
wget google.com
```

即可完成对当前的终端窗口（shell 会话）设置可用于代理的环境变量。然后输入遵守 `$http_proxy` 或 `$https_proxy` 变量的命令即可。

要取消设置变量，只要关闭当前的终端窗口即可，或者输入：

```shell
unset http_proxy&&unset https_proxy
```

对于需要 `sudo` 的命令，如 `pacman`，可先使用下述命令登录 root 账户，然后再设置环境变量。

```shell
su - #或 sudo su
```

## Pacman 命令

在 archlinux 上安装的软件都通过 Pacman 来进行管理[^1]。

为了使用 Pacman 额外的命令需要先安装 `pacman-contrib`:

```shell
sudo pacman -S pacman-contrib
```

常用命令如下：

```
sudo pacman -S package_name # 安装软件包
pacman -Ss # 在同步数据库中搜索包，包括包的名称和描述
sudo pacman -Syyu # 升级系统。-yy：标记强制刷新、-u：标记升级动作
sudo pacman -Rns package_name # 删除软件包，及其所有没有被其他已安装软件包使用的依赖包
sudo pacman -R package_name # 删除软件包，保留其全部已经安装的依赖关系
pacman -Qi package_name # 检查已安装包的相关信息。-Q：查询本地软件包数据库
pacman -Qdt # 找出孤立包。-d：标记依赖包、-t：标记不需要的包、-dt：合并标记孤立包
sudo pacman -Rns $(pacman -Qtdq) # 删除孤立包
sudo pacman -Fy # 更新命令查询文件列表数据库
pacman -F some_command # 当不知道某个命令属于哪个包时，用来在远程软件包中查询某个命令属于哪个包（即使没有安装）
pactree package_name # 查看一个包的依赖树
```

## Flatpak 初始化

安装 flatpak：

```
sudo pacman -S flatpak
```

加入用户组（运行下述命令后，需要重新登陆系统）：

```
sudo usermod -aG flatpak $USER
```

设置 flatpak 镜像源：

```
sudo flatpak remote-modify flathub --url=https://mirror.sjtu.edu.cn/flathub
```

如果出现错误可尝试：

```
wget https://mirror.sjtu.edu.cn/flathub/flathub.gpg
```

```
sudo flatpak remote-modify --gpg-import=flathub.gpg flathub
```

如果您中断了某次安装，重新下载可能会出现找不到文件的问题。您可以使用 `flatpak repair` 解决相关的问题[^2]。

Flatpak 期望窗口管理器（如 KDE、Gnome 等）尊重 `XDG_DATA_DIRS` 环境变量来发现应用程序。此变量由脚本 `/etc/profile.d/flatpak.sh` 设置。更新环境可能需要重新启动会话（重新登陆系统）。如果启动器不支持 `XDG_DATA_DIRS`，你可以运行如下命令（然后重新登陆系统）：

```
cd /etc/profile.d/ && sudo sh ./flatpak.sh
```

### 简易使用

!!! note
    请不要使用 `sudo` 命令运行 `flatpak`。以免导致一些预期之外的问题。  
    你可以直接通过浏览器访问 [Flathub](https://flathub.org/home) 获取特定的 flatpak 应用的安装与启动命令。  

查看用户手册：

```
flatpak --help
```

检索软件：

```
flatpak search [软件名称] #如： flatpak search atom
```

安装软件：

```
flatpak install [软件名称] #如： flatpak install Atom
```

运行软件：

```
flatpak run [应用 ID] #如： flatpak run io.atom.Atom
```

查看已安装的软件：

```
flatpak list
```

卸载软件：

```
flatpak uninstall [软件名称]
```

更新软件：

```
flatpak update [软件名称] #如果不指定特定应用，则会检查更新全部命令
```

## 新建开始菜单启动项

建议将 clash for windows 的文件全部存放到 `~/bin/cfw` 中。

新建一个名为 `cfw.desktop` 的纯文本文件，使用 `kwrite` 打开此文件并写入：

```
[Desktop Entry]
Type=Application
Name=Clash for windows
Comment=Network Porxy Tool
Exec=/home/deck/bin/cfw/cfw
Icon=
Terminal=false
Categories=Network;
```

然后将此文件移动到 `/home/deck/.local/share/applications` 中。

## 简化设置代理变量

使用 `kwrite` 打开 `/home/deck/.bashrc`，在文件末尾输入：

```sh
export PATH=$PATH:/home/deck/bin
#add a specific fodler to $PATH

alias sudo="sudo "
#let shell recognizes alias of commands behind sudo.

alias font-ref="fc-cache -fv"
#For refreshing the fonts cache, used when you add new fonts to system.
#   If you need to install fonts manually, you can place the fonts in
#the folder(e.g. ~/.fonts) indicated by the output of the font refresh
#command above.

alias proxy="export http_proxy=http://127.0.0.1:7890; export https_proxy=$http_proxy;"
#Setting a quick commad for proxy.
```

保存并关闭文件，然后在终端中执行：

```
source ~/.bashrc
```

然后你就可以直接在终端运行 `proxy` 命令，设置代理。

[^1]: 本节内容来源于：https://arch.icekylin.online/advanced/system-ctl.html#pacman-%E5%8C%85%E7%AE%A1%E7%90%86
[^2]: 节选自：https://mirror.sjtu.edu.cn/docs/flathub