---
comments: true
tags:
  - 杂谈
  - Linux
  - openSUSE
---

# Tumbleweed 配置小记（一）

!!! info "更新时间"

    2023-10-09

!!! warning "注意"

    这份文档记录的并不是标准的 openSUSE 安装流程。

## 准备

[下载 ISO 文件]，例如：

[下载 ISO 文件]: ./../guide/misc/mirror-site.md

```
wget https://opentuna.cn/opensuse/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso
```
```
wget https://opentuna.cn/opensuse/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso.sha256
```
```
wget https://opentuna.cn/opensuse/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso.sha256.asc
```

下载完毕后，[校验 hash 和签名]。

[校验 hash 和签名]: ./../guide/install/prepare.md

烧录工具可以使用 [Rufus]、[ventoy] 或 [Fedora Media Writer]。

[Rufus]: https://rufus.ie
[Fedora Media Writer]: https://www.fedoraproject.org/en/workstation/download/
[ventoy]: https://www.ventoy.net

## 安装系统

启动安装介质后，不必联网

|分区|大小|格式|备注&操作|
|:---|:---:|:---:|---|
|/boot/efi|256MiB|efi|挂载，格式化|
|SWAP|16GiB[^ram]|swap|挂载，格式化|
|/|40GiB|btrfs|格式化，并启用系统快照。|
|/home|-|xfs|仅挂载，手动清理文件|
|/bt|-|xfs|仅挂载，bt 做种文件存储分区|

[^ram]: 本机系统为 16GB RAM。

- 需要删除的软件包：`ibus`、`fcitx`、`opensuse-welcome`、`kompare`、`discover`、`PackageKit`、`konversation`、`kmousetool`、`vlc`、`skanlite`、`openSUSE-repos-*`
- 需要禁用的模组：`pattern:games`、`pattern:kde_pim`
- 需要安装的软件包：`git-core`、`flatpak`、`kleopatra`

如果忘记了重命名旧的用户文件夹，那么无需新建普通用户。

## 初次启动

### 修改软件源

重装系统后，使用 `root` 身份登陆系统。将旧用户文件夹重命名，然后使用 KDE 系统设置新建一个普通用户。新旧用户的 ID 应保持一致。

然后将新用户添加至用户组：

```
usermod -aG wheel poplar && usermod -aG flatpak poplar
```

再以新用户身份登陆系统。

禁用全部的软件源：

```
sudo zypper mr -da
```

添加第三方软件源并更新系统（可选），例如：

```
sudo zypper ar -fcg https://opentuna.cn/opensuse/tumbleweed/repo/oss/ opentuna-oss
```
```
sudo zypper ar -fcg https://opentuna.cn/opensuse/tumbleweed/repo/non-oss/ opentuna-non-oss
```
```
sudo zypper ref && sudo zypper dup -y
```

修补 YaST2 GUI 无法启动的 BUG：

```
sudo zypper in which
```

重启系统。

### 安装多媒体解码器

```
sudo zypper ar -cfp 90 https://mirrors.aliyun.com/packman/suse/openSUSE_Tumbleweed/ packman
```
```
sudo zypper refresh && sudo zypper dist-upgrade --from packman --allow-vendor-change
```
```
sudo zypper install --from packman ffmpeg gstreamer-plugins-{good,bad,ugly,libav} libavcodec-full
```

重启系统。

### 安装 proxychains-ng

设置 proxychains.conf：

```
sudo zypper in proxychains-ng
```

编辑配置文件：

```
sudo nano /etc/proxychains.conf
-------------------------------
quiet_mode
http  127.0.0.1 7890
```

### 迁移旧数据

需要重新装入的文件：

```
/home/poplar/{公共,模板,图片,文档,下载,音乐,bin,game}

/home/poplar/.mozilla
/home/poplar/.steam
/home/poplar/.var
/home/poplar/.bashrc
/home/poplar/.gitconfig
/home/poplar/.steampath
/home/poplar/.steampid

/home/poplar/.config/aria2
/home/poplar/.config/deadbeef
/home/poplar/.config/fcitx
/home/poplar/.config/fcitx5
/home/poplar/.config/goldendict
/home/poplar/.config/keepassxc
/home/poplar/.config/mpv
/home/poplar/.config/peazip
/home/poplar/.config/pip
/home/poplar/.config/qBittorrent
/home/poplar/.config/RSS Guard 4
/home/poplar/.config/VirtualBox

/home/poplar/.local/share/applications
/home/poplar/.local/share/fcitx5
/home/poplar/.local/share/flatpak
/home/poplar/.local/share/fonts
/home/poplar/.local/share/goldendict
/home/poplar/.local/share/kleopatra
/home/poplar/.local/share/qBittorrent
/home/poplar/.local/share/Steam
/home/poplar/.local/share/steamtricks
```

游戏数据文件：

```
/home/poplar/.renpy
/home/poplar/.config/tits
/home/poplar/.local/share/godot
/home/poplar/.local/share/Mindustry
```

## 初始化

### 设置主机名

```
sudo hostnamectl set-hostname --pretty "White-Poplar's Laptop"
```
```
sudo hostnamectl set-hostname --static c004-h0
```

### 安装软件

|包名/名称|源|描述|子包/Flatpak 包名|
|---|---|---|---|
|`keepassxc`|发行版仓库|密码管理|
|`aria2c`|发行版仓库|下载工具|
|`mpv`|发行版仓库|多媒体播放器|
|`opi`（可选）|发行版仓库|OBS 仓库助手|
|64Gram Desktop|Flatpak Remote|即时通讯软件|`io.github.tdesktop_x64.TDesktop`|
|`gimp`|发行版仓库|图片编辑|
|`deadbeef`|发行版仓库|音乐播放器|
|`fcitx5`|发行版仓库|输入法|`fcitx5-configtool`、`fcitx5-chinese-addons`|
|`FreeFileSync`|发行版仓库|文件同步/比对|
|Czkawka|Flatpak Remote|文件查重工具|`com.github.qarmin.czkawka`|
|Calibre|Flatpak Remote|电子书阅读器|`com.calibre_ebook.calibre`|
|Draw.io|Flatpak Remote|思维导图工具|`com.jgraph.drawio.desktop`|
|Crow Translate|Flatpak Remote|翻译/OCR|`io.crow_translate.CrowTranslate`|
|`virtualbox`|发行版仓库|虚拟机|
|`goldendict-ng`|发行版仓库|字典|`goldendict-ng-lang`|
|Visual Studio Code|Microsoft|源代码编辑器|使用 `*.tar.gz` 便携版文件包<br /><https://code.visualstudio.com/Download>|
|`kfind`|发行版仓库|文件查找工具|
|`peazip`|发行版仓库|解压缩管理工具|`peazip-kf5`|
|`rssguard`|发行版仓库|RSS 订阅管理|
|`steam`|发行版仓库|数字游戏平台|

```
sudo zypper in keepassxc mpv aria2c gimp deadbeef fcitx5 fcitx5-configtool fcitx5-chinese-addons virtualbox goldendict-ng goldendict-ng-lang FreeFileSync kfind peazip peazip-kf5 rssguard steam
```

加入新的用户组：

```
sudo usermod -aG vboxusers $USER
```

添加 NVIDIA 软件源（可选）：

```
sudo zypper ar --refresh https://download.nvidia.com/opensuse/tumbleweed NVIDIA && sudo zypper ref
```

补齐系统推荐的依赖软件包：

```
sudo zypper inr
```

然后重启系统。

### KDE 相关

将 `~/公共` 文件夹下的图标包解压，然后移动到 `/usr/share/icons`

- <https://github.com/vinceliuice/Qogir-icon-theme>
    - <https://www.pling.com/p/1366182>
- <https://github.com/vinceliuice/Tela-icon-theme>

主题设置为 **Breeze 微风** 或者 **Breezy 微风深色**。

在 **KDE 系统设置** -> **开机与关机** -> **桌面会话** 中，将 **登陆时自动启动应用程序** 设置为 **空会话**。

### 安装相关的 Flatpak 软件包

添加仓库：

```
flatpakx remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

安装软件：

```
flatpakx install flathub com.calibre_ebook.calibre
```
```
flatpakx install flathub com.jgraph.drawio.desktop
```
```
flatpakx install flathub com.github.qarmin.czkawka
```
```
flatpakx install flathub io.github.tdesktop_x64.TDesktop
```
```
flatpakx install flathub io.crow_translate.CrowTranslate
```

### 启动 tlp（可选）

```
sudo systemctl enable tlp --now
```
```
tlp-stat -s #依照提示，手动屏蔽相关内容
```
```
sudo systemctl status power-profiles-daemon.service tlp.service
```
```
sudo systemctl mask power-profiles-daemon.service
```

### 配置 SUSE-prime（可选）

```
sudo zypper in bbswitch-kmp-default
```