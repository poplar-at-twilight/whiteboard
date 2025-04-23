---
tags:
  - Linux
  - 笔记
---

# 初次启动

## 移动/恢复备份文件

将个人文件复制到新的 `$USER` 文件夹中。

```
/home/poplar/bin
/home/poplar/Desktop
/home/poplar/Documents
/home/poplar/Downloads
/home/poplar/Misc
/home/poplar/Others
/home/poplar/Pictures

/home/poplar/.config/audacious
/home/poplar/.config/fcitx5
/home/poplar/.config/google-chrome
/home/poplar/.config/MangoHud
/home/poplar/.config/mpv
/home/poplar/.config/pip
/home/poplar/.config/qBittorrent

/home/poplar/.local/share/fcitx5
/home/poplar/.local/share/FlClash
/home/poplar/.local/share/fonts
/home/poplar/.local/share/konsole
/home/poplar/.local/share/qBittorrent
/home/poplar/.local/share/plasma-systemmonitor

/home/poplar/.var

/home/poplar/.bashrc
/home/poplar/.gitconfig
```

- 不要备份 flatpak 和 steam 的应用数据文件夹。
- 从其他发行版更新至 Fedora 时，bashrc 的内容应接在文件的末尾，而非直接覆盖。

## 设置主机名

```
set-hostname
```

## 安装基本工具

- [x] [FlClash]

连接网络，使用 dnf 安装该 rpm 软件包。

[FlClash]: https://github.com/chen08209/FlClash

安装输入法、git-core 和 keepassxc：

```
sudo dnf in fcitx5 fcitx5-chinese-addons git-core keepassxc
```

然后在 KDE 系统设置中，将虚拟键盘设置为 Fcitx5。

## 清理无用软件包

- akregator
- plasma-discover
- dragon
- elisa-player
- mediawriter
- firefox
- plasma-welcome
- kmouth
- kaddressbook
- kamoso
- kdeconnectd
- kdebugsettings
- kgpg
- kmahjongg
- kmail
- kmines
- kolourpaint
- korganizer
- kpat
- krdc
- krfb
- neochat
- qrca
- skanpage
- PackageKit
- ibus

```
sudo dnf rm akregator plasma-discover dragon elisa-player mediawriter firefox plasma-welcome kmouth kaddressbook kamoso kdeconnectd kdebugsettings kgpg kmahjongg kmail kolourpaint korganizer kpat krdc krfb neochat qrca skanpage PackageKit ibus
```

自动移除不再使用的软件包：

```
sudo dnf autoremove
```

- 可用 `sudo dnf clean all` 清除全部缓存。
- 可用 `dnfx` 连接代理更新。
- 详见 `dnf --help`。

## 更新并重启

```
sudo dnf up
```

----

## 安装软件包

### rpmfusion

```
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

### Chrome

```
sudo dnf install fedora-workstation-repositories
```

```
sudo dnf config-manager setopt google-chrome.enabled=1
```

```
sudo dnf install google-chrome-stable
```

### 多媒体播放器

```
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
```

```
sudo dnf in audioucs mpv
```

### flatpak

```
sudo dnf in flatpak
```

```
flatpak-add
```

```
sudo usermod -aG flatpak $USER
```

删除系统的软件源：

```
flatpak remote-delete fedora
```

设置变量：

```
flatpak override --env=QT_IM_MODULE= --user
flatpak override --env=GTK_IM_MODULE= --user
```

需要安装的软件：

- `org.telegram.desktop`
- `cc.spek.Spek`

### KVM

```
sudo dnf in @virtualization
```
```
sudo usermod -aG libvirt $USER
```
```
sudo systemctl enable libvirtd --now
```

重启系统。

### 其他

- gimp
- goverlay
    - 详见 `man mangohud`
- kleopatra
- goldendict-ng
    - 如果出现问题则换成 flatpak 版
- steam
    - 更改默认界面语言
    - 关闭着色器缓存
    - 更改默认打开的页面
    - 添加新的库
    - 启用 steam proton 兼容层
- jpegoptim

```
sudo dnf in kleopatra goverlay gimp goldendict-ng steam jpegoptim
```

----

## 散装软件包

### FreeFileSync

直接安装开发者提供的二进制文件。

- <https://freefilesync.org/download.php>

### vscodium & pandoc & mangohud & Goverlay

参考 [软件包](./../note-openSUSE/4-software.md)。