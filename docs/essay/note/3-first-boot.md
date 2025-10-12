---
tags:
  - Linux
  - 笔记
---

# 初次启动

## 恢复备份文件

将个人文件复制到新的 `$USER` 文件夹中。一般需要：

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
/home/poplar/.config/chromium
/home/poplar/.config/goldendict
/home/poplar/.config/htop
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
/home/poplar/.local/share/TelegramDesktop

/home/poplar/.var

/home/poplar/.bashrc
/home/poplar/.gitconfig
```

- 不要备份 flatpak 和 steam 的应用文件夹（存在 sockets 和大量小文件）。
- 从其他发行版更新至 Fedora 时，旧版 `~/.bashrc` 的内容应接在文件的末尾，而非直接覆盖。

## 更改防火墙

打开 `firewall-config`，将默认区域从 `fedora-workstation` 修改为 `public`，并设置为 **永久** 配置。

## 设置主机名

```
set-hostname
```

## 音量控制故障

如果播放器音量大小与系统设置的音量值不呈线性变化，则使用 `alsamixer` 将 `Base Speaker` 的值降到 0。

## 安装基本工具

### 网络

- [x] [FlClash]

连接网络，使用 dnf 安装该 rpm 软件包及依赖。

[FlClash]: https://github.com/chen08209/FlClash

### 其他

安装输入法、git-core 和 keepassxc：

```
sudo dnf in fcitx5 fcitx5-chinese-addons git-core keepassxc
```

### Fcitx5

在 KDE 系统设置中，将虚拟键盘设置为 Fcitx5。

Fcitx5 的主题使用 [Fluent-fcitx5]。

[Fluent-fcitx5]: https://github.com/Reverier-Xu/Fluent-fcitx5

Fcitx5 的自定义词库文件（`*.dict`）：

- [felixonmars/fcitx5-pinyin-zhwiki]
- [wuhgit/CustomPinyinDictionary]

!!! info "注意"

    词库维护者会把新旧版词库放在同一个 release 中。

[felixonmars/fcitx5-pinyin-zhwiki]: https://github.com/felixonmars/fcitx5-pinyin-zhwiki
[wuhgit/CustomPinyinDictionary]: https://github.com/wuhgit/CustomPinyinDictionary

词库文件夹：`~/.local/share/fcitx5/pinyin/dictionaries/`

附注：

- 云输入法可以使用 Google 源，但需要设置代理。
- 跟换皮肤后，如果没有正常地显示，可以重启一下系统。

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
- plasma-browser-integration

```
sudo dnf rm akregator plasma-discover dragon elisa-player mediawriter firefox plasma-welcome kmouth kaddressbook kamoso kdeconnectd kdebugsettings kgpg kmahjongg kmail kmines kolourpaint korganizer kpat krdc krfb neochat qrca skanpage PackageKit ibus plasma-browser-integration
```

自动移除不再使用的软件包：

```
sudo dnf autoremove
```

- 可用 `sudo dnf clean all` 清除全部缓存。
- 详见 `dnf --help`。

移除 geoclue2 服务：

```
sudo rm /etc/xdg/autostart/geoclue-demo-agent.desktop
```

## 更新并重启

```
sudo dnf up
```

----

## 安装软件包

为 libreoffice 安装中文语言包：

!!! Tips

    当不需要安装弱依赖时，可以选择使用 `--setopt=install_weak_deps=False` 选项。

```
sudo dnf in libreoffice-langpack-zh-Hans --setopt=install_weak_deps=False
```

### RPMFusion

```
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

切换软件包供应商：

```
sudo dnf swap mesa-va-drivers mesa-va-drivers-freeworld
sudo dnf swap mesa-vdpau-drivers mesa-vdpau-drivers-freeworld
sudo dnf swap mesa-vulkan-drivers mesa-vulkan-drivers-freeworld
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
```

### 浏览器

如果无法使用 fcitx 输入中文，在 `chrome://flags` 中打开 `#wayland-text-input-v3`，并将 `--ozone-platform-hint=auto --enable-wayland-ime` 添加至应用程序的环境变量中。

#### Chrome

```
sudo dnf install fedora-workstation-repositories
```

```
sudo dnf config-manager setopt google-chrome.enabled=1
```

!!! Tips

    将上述命令的布尔值改为 `0` 可以禁用此第三方软件源。

```
sudo dnf install google-chrome-stable
```

#### Chromium

```
sudo dnf install chromium
```

### 多媒体播放器

```
sudo dnf in audacious mpv
```

### Flatpak

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

清理不再需要的库：

```
flatpak uninstall --unused
```

卸载时一并清理应用数据：

```
flatpak uninstall <package id> --delete-data
```

#### fcitx5 皮肤没有正确显示

关闭所有 flatpak 应用，然后运行：

```shell
flatpak override --env=QT_IM_MODULE= --user
flatpak override --env=GTK_IM_MODULE= --user
```

#### 应用 GTK 主题与当前系统主题不匹配

使用 Flatseal 编辑应用，使其能访问主题文件夹（`/usr/share/themes`），然后为该应用设置如下环境变量：

```
GTK_THEME=Adwaita-dark
```

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

重启系统，另见：[KVM 备忘录]。

[KVM 备忘录]: ./../linux/1-kvm.md

### 其他

- `gimp`
- `goverlay`
    - 详见 `man mangohud`
- `kleopatra`
- `goldendict-ng`
    - 如果出现问题则换成 flatpak 版
- `steam`
    - 更改默认界面语言
    - 关闭着色器缓存
    - 更改默认打开的页面
    - 添加新的库
    - 启用 steam proton 兼容层
    - steam 会默认优先运行游戏的 Linux 原生版本。如果出现性能问题，请强制使用 steam proton 兼容工具。
    - 如果存在连接性问题，则使用 `steam-proxy` 命令。
    - 如果无法正常缩放，则设置变量：  
    ```
    STEAM_FORCE_DESKTOPUI_SCALING=1.5
    ```
- `jpegoptim`
- `qbittorrent`
- `pandoc`
- `telegram-desktop`

```
sudo dnf in kleopatra goverlay gimp goldendict-ng steam jpegoptim qbittorrent pandoc telegram-desktop
```

!!! info

    Fedora KDE 默认使用 Kwrite，它比 Kate 简洁轻便很多。不太需要更换。

----

## 散装软件包

### VSCodium

下载文件：<https://github.com/VSCodium/vscodium/releases>

在与 `VSCodium*.tar.gz` 同级文件夹中新建 `data/extensions` 和 `data/user-data`。

!!! Tips

    右键点击 KDE 开始菜单，选择 **编辑应用程序**，可以快速新建一个 desktop 文件。

desktop 文件模板：

```shell
[Desktop Entry]
Categories=Development;
Comment[zh_CN]=
Comment=
Exec=/home/poplar/bin/codium/codium --ozone-platform-hint=auto --enable-wayland-ime --wayland-text-input-v3
GenericName[zh_CN]=
GenericName=
Icon=/home/poplar/bin/codium/data/paulo22s.png
MimeType=
Name[zh_CN]=VSCodium
Name=VSCodium
Path=/home/poplar/bin/codium
PrefersNonDefaultGPU=false
StartupNotify=false
Terminal=false
TerminalOptions=
Type=Application
X-KDE-SubstituteUID=false
X-KDE-Username=
```

Logo 文件可从 [VSCodium/icons] 仓库下载获得。

[VSCodium/icons]: https://github.com/VSCodium/icons

字体设置：

```
'Noto Sans Mono', 'Noto Sans SC', monospace
```

常用扩展列表：

- `MS-CEINTL.vscode-language-pack-zh-hans`
- `GitHub.vscode-pull-request-github`
- `GitHub.github-vscode-theme`
- `shd101wyy.markdown-preview-enhanced`
- `PKief.material-icon-theme`
- `alefragnani.project-manager`
- `ms-python.python`
- `AaaaronZhou.vscode-auto-light-dark-theme`

图标另见： <https://github.com/VSCodium/icons>

更新脚本 `update-code` 另见 [shell-script]。

[shell-script]: ./4-shell-script.md

### FreeFileSync

直接安装开发者提供的二进制文件。

- <https://freefilesync.org/download.php>

### mangohud & Goverlay

对于 steam 游戏，使用 `mangohud %command%`。使用说明详见：`man mangohud`

注意：

1. 不要在游戏启动时让 mangohud 读取游戏的平均帧和 1% low 帧
1. 使用 Goverlay 时，如果有按钮被遮挡，可以把显示器缩放比例从 150% 调整为 100%

### nano

如果 `nano` 没有启用语法高亮，则在用户目录下添加下列配置文件：

```shell
poplar@Greysia:~> cat .nanorc
include /usr/share/nano/*.nanorc
```

### Czkawka

下载 `linux_czkawka_gui`

- 地址：<https://github.com/qarmin/czkawka/releases>