---
tags:
  - Linux 简明指南
  - 工具集
  - 教程
  - 网络浏览器
  - 下载管理器
  - 即时通讯
  - 游戏
  - 多媒体
  - 文档办公
---

# 实用应用程序列表

本文列举一些常用、实用的应用程序。

## 网络浏览器

Firefox 是 openSUSE 与 Fedora 的默认网络浏览器。

你可以在 Firefox 的官方[下载页面](https://www.mozilla.org/en-US/firefox/all/#product-desktop-release)中找到任意版本，任意语言和地区的安装包或压缩包文件。

在将文件下载至本地后（例如将文件下载至 `~/Download`），你可以根据[官方指南](https://support.mozilla.org/en-US/kb/install-firefox-linux)执行以下操作：

1. 移动到下载文件夹，并将压缩包解压：  
    ```
    cd ~/Downloads && tar xjf firefox-*.tar.bz2
    ```
2. 然后将解压好的文件夹移动到 `/opt` 目录下：  
    ```
    mv firefox /opt
    ```
3. 创建一个符号链接：  
    ```
    ln -s /opt/firefox/firefox /usr/local/bin/firefox
    ```
4. 下载一个 Desktop 文件用于在图形界面中创建程序启动按钮：  
    ```
    wget https://raw.githubusercontent.com/mozilla/sumo-kb/main/install-firefox-linux/firefox.desktop -P /usr/local/share/applications
    ```

### Tor 浏览器

[Tor 浏览器](https://en.wikipedia.org/wiki/Tor_(network)#Tor_Browser)是一个支持[洋葱路由协议](https://en.wikipedia.org/wiki/Onion_routing)，可实现匿名通讯的开源软件。

通过 Flatpak 安装：

```
flatpak install flathub com.github.micahflee.torbrowser-launcher
```

在 Fedora 上安装：

```
sudo dnf install torbrowser-launcher
```

在 openSUSE 上安装：

```
sudo zypper install torbrowser-launcher
```

### Google Chrome

!!! note "注意"

    如果你需要使用 Chrome 的非稳定版版，你需要将 `google-chrome-stable` 替换为 `google-chrome-unstable` （金丝雀版）或者 `google-chrome-beta`（beta 版）

在 openSUSE 上安装谷歌 chrome：

1. 添加仓库：  
    ```
    sudo zypper ar http://dl.google.com/linux/chrome/rpm/stable/x86_64 google-chrome
    ```
2. 获取公钥：  
    ```
    wget https://dl.google.com/linux/linux_signing_key.pub
    ```
3. 导入公钥：  
    ```
    sudo rpm --import linux_signing_key.pub
    ```
4. 安装 Google chrome 稳定版。
    ```
    sudo zypper ref && sudo zypper in google-chrome-stable
    ```

在 Fedora 上安装谷歌 chrome：

1. 添加第三方仓库：
    ```
    sudo dnf install fedora-workstation-repositories
    ```
2. 激活 Google Chrome 仓库：
    ```
    sudo dnf config-manager --set-enabled google-chrome
    ```
3. 安装 chrome 稳定版：
    ```
    sudo dnf install google-chrome-stable
    ```

### Chromium

!!! note "版权分发限制"

    - 由于版权限制，openSUSE 和 Fedora 不能分发存在版权争议的内容。如果你需要浏览器支持一些闭源格式（如 H.264、AAC 或其他 DRM 保护内容），你需要手动安装闭源组件。
    - RPMfusion 已经提供一个包含完整组件的 `chromium-freeworld`。

!!! bug "数据同步"

    - 谷歌已经禁止 chromium 和基于 chromium 的第三方浏览器读取谷歌的数据，所以 chromium 不能同步你原有的谷歌浏览器数据。

Chrome 是基于开源浏览器 chromium 而搭建的闭源浏览器。如果你需要使用 Chromium，请依照一下步骤进行操作：

在 openSUSE 上安装 chromium：

```
sudo zypper in chromium 
```

安装闭源组件（可选）：

```
sudo zypper in chromium-bsu chromium-ffmpeg-extra chromium-plugin-widevinecdm
```

在 Fedora 上安装 chromium：

```
sudo dnf in chromium
```

安装由 RPMFusion 提供，适用于 Fedora 的完整版 chromium（可选）：

```
sudo dnf in chromium-freeworld
```

### Flatpak 版

你可以通过 Flatpak 安装 chrome/chromium：

安装谷歌浏览器：

```
flatpak install flathub com.google.Chrome #谷歌浏览器稳定版
```

```
flatpak install flathub com.google.ChromeDev #谷歌浏览器开发版
```

安装 chromium：

```
flatpak install flathub org.chromium.Chromium
```

安装 ungoogled-chromium：

!!! note "说明"

    ungoogled-chromium 是一个由社区维护，默认禁用谷歌隐私追踪的开源浏览器。详细说明、修改内容以及潜在性问题详见[此处](https://github.com/Eloston/ungoogled-chromium/blob/master/README.md)。

```
flatpak install flathub com.github.Eloston.UngoogledChromium
```

----

## 下载管理器

### 通用下载器

`wget` 是许多 Linux 系统内置的一个下载工具，你可以使用下列命令查阅使用手册：

```
$ wget --help
```

[Aria2](https://aria2.github.io/)

```
sudo dnf in aria2
sudo zypper in aria2
```

[Motrix](https://motrix.app/) 是一个基于 aria2 开发通用下载器，支持 HTTP、FTP 和 BT 磁力链等下载内容。除了 Flatpak 安装方式，motrix 也有提供 appimage 供下载使用。

```
flatpak install flathub net.agalwood.Motrix
```

### BT 下载器

[qBittorrent](https://www.qbittorrent.org/) 是一个流行的 bt 下载做种工具。

!!! tip "一些建议"

    - 如果你对于反吸血，屏蔽迅雷有需求，可以试试 [qBittorrent-enhanced-edition](https://github.com/c0re100/qBittorrent-Enhanced-Edition/releases)。  
    - 适用于 openSUSE 的 qBittorrent-enhanced-edition 的 OBS 仓库详见[此处](https://build.opensuse.org/package/show/home:opensuse_zh/qBittorrent-Enhanced-Edition)。
    - 有关如何使用 tracker 加速 BT 下载的指南详见[此处](https://trackerslist.com/)。

```
sudo dnf in qbittorrent
sudo zypper in qbittorrent
```

[Transmission](https://transmissionbt.com/) 是一个历史悠久的 bt 下载工具

```
sudo dnf in transmission
sudo zypper in transmission
```

----

## 即时通讯

### QQ

[Icalingua++](https://github.com/Icalingua-plus-plus/Icalingua-plus-plus) 是一个由社区开发的会话前端框架，支持腾讯 QQ。

### telegram

!!! info "关于电报"

    有相当一部分的 Linux 用户会汇聚在各种 Telegram 群组之中；单独的 qq 或微信群组则很少见。

```
sudo dnf in telegram-desktop
sudo zypper in telegram-desktop
```

Linux 中文本地社区群组：

|名称|ID|
|---|---|
|Fedora 中文用户组|@fedorazh|
|openSUSE 中国|@opensuse_cn|
|gentoo 中文社区|@gentoo_zh|
|Arch Linux 中文|@archlinuxcn_group|
|Debian 中文频道|@debianzh|

Linux 国际社区群组：

|名称|ID|
|---|---|
|Fedora Linux Gateway|@fedora|
|openSUSE in Telegram|@opensuse_telegram|
|Arch|@archlinuxgroup|

----

## 游戏平台

### Steam

你需要在 steam 的设置中启动 Proton 来游玩 Windows 游戏。

```
sudo dnf in steam
sudo zypper in steam
```

### GOG.com

[GOG.com](https://www.gog.com/) 虽然没有官方 Linux 客户端，但它售卖的一部分游戏支持 Linux（默认的系统是 Ubuntu）

----

## 多媒体相关

### 图片编辑

[GIMP](https://www.gimp.org/) 是一个功能和 Photoshop 相似的开源图片编辑软件，基于 GTK 开发。

```
sudo dnf in gimp     #在 Fedora 上安装
sudo zypper in gimp  #在 openSUSE 上安装
```

[Krita](https://krita.org/) 是基于 Qt 开放，著名的开源绘画软件，它也支持简单的图片编辑功能。

```
sudo dnf in krita     #在 Fedora 上安装
sudo zypper in krita  #在 openSUSE 上安装
```

### 视频编辑

[Blender](https://www.blender.org/) 是一个集动画电影制作、视觉特效、3D 打印、三维/二维建模、动态图形、交互式 3D 应用和 VR 等多项领域为一体的知名开源软件。

```
sudo dnf in blender
sudo zypper in blender
```

[Shotcut](https://www.shotcut.org/) 是由 [MIT 多媒体框架](https://en.wikipedia.org/wiki/Media_Lovin%27_Toolkit)开发者开发的一款基于 MIT 多媒体框架的非线性视频编辑软件。

```
sudo dnf in shotcut 
sudo zypper in shotcut
```

[Kdenlive](https://kdenlive.org/) 是一个基于 Qt 和 MIT 多媒体框架开发的非线性视频编辑软件。

```
sudo dnf in kdenlive
sudo zypper in kdenlive
```

### 屏幕录制

[OBS](https://obsproject.com/) 是一款用于视频录制和直播推流的开源软件。

```
sudo dnf in obs-studio  
sudo zypper in obs-studio 
```

### 音频编辑

[Audacity](https://www.audacityteam.org/) 是一个用于音频录制与音频编辑的开源软件。

```
sudo dnf in audacity  
sudo zypper in audacity 
```

### 字幕编辑

[Aegisub](https://github.com/Aegisub/Aegisub) 是一个跨平台的高级字幕编辑工具。

```
sudo dnf in aegisub 
sudo zypper in aegisub  
```

### 视频播放器

[MPV](https://mpv.io/) 是一个备受 Linux 用户欢迎，高度可定制的多媒体播放器。它同时是很多播放器的后端。

```
sudo dnf in mpv
sudo zypper in mpv
```

[VLC](https://www.videolan.org/vlc/) 是另一个广为流行，跨平台，兼容几乎绝大部分多媒体格式（不仅仅是视频）的开源多媒体播放器。

```
sudo dnf in vlc
sudo zypper in vlc
```

[SMplayer](https://www.smplayer.info/en/downloads) 是一个基于 mpv 的视频播放器，它简单化了 mpv 的复杂配置，并提供一个良好的开箱即用体验。

```
sudo dnf in smplayer smplayer-themes  #安装播放器本体及播放器的扩展皮肤包
sudo zypper in smplayer smplayer-themes
```

### 音频播放器

[Deadbeef](https://deadbeef.sourceforge.io/) 是一个在 HiFi 社区中流行，可自定义的开源音频播放器。

```
sudo dnf in deadbeef
sudo zypper in deadbeef
```

[Audacious](https://audacious-media-player.org/) 是一个开源的音频播放器。

```
sudo dnf in audacious
sudo zypper in audacious
```

### 流媒体播放器

开源的 Apple Music 播放器：

```
flatpak install flathub sh.cider.Cider
```

网易云音乐：

```
flatpak install flathub com.netease.CloudMusic
```

网易云音乐 GTK 版：

```
flatpak install flathub com.github.gmg137.netease-cloud-music-gtk
```

QQ 音乐：

```
flatpak install flathub com.qq.QQmusic
```

Sportily：

```
flatpak install flathub com.spotify.Client
```

Google Play Music：

```
flatpak install flathub com.googleplaymusicdesktopplayer.GPMDP
```

----

## 文档办公

### WPS Office

WPS Office 套件支持 Linux 系统，并且是 Microsoft office 的优秀替代品。

你可以直接在 [WPS For Linux 官网](https://www.wps.com/office/linux/)下载适用于 Fedora 的 rpm 安装包，然使用 `rpm`  命令安装 WPS:

```
$ sudo rpm -i wps-office-*.rpm
```

或者通过 flatpak 安装：

!!! bug "关于中文支持"

    现有的 Flatpak 版是 wps 国际版，不包含中文语言包。

```
flatpak install flathub com.wps.Office
```

openSUSE 用户可通过 OBS 仓库安装：

```
sudo zypper addrepo https://download.opensuse.org/repositories/home:fusionfuture:office/openSUSE_Tumbleweed/home:fusionfuture:office.repo

sudo zypper refresh && sudo zypper install wps-office
```

#### 字体缺失问题

由于许可证的原因，WPS 不能携带一些 windows 字体，你需要自行去搜索下载相关的字体文件。你可以在 [BannedPatriot/ttf-wps-fonts](https://github.com/BannedPatriot/ttf-wps-fonts) 下载缺失的字体。

### Libreoffice

Libreoffice 是一套开源的办公软件套件：

```
sudo zypper install libreoffice
sudo dnf install libreoffice
```

通过 flatpak 安装：

```
flatpak install flathub org.libreoffice.LibreOffice
```

### E-mail

[Thunderbird](https://www.thunderbird.net/en-US/) 和 [evolution](https://wiki.gnome.org/Apps/Evolution) 都是流行的 Linux 邮件客户端。你可以任选一个安装至你的电脑。

!!! note "注意"

    如果你使用 `evolution` ，请再安装 `evolution-ews` 插件。

在 openSUSE 上安装邮件客户端：

```
sudo zypper in MozillaThunderbird
```
```
sudo zypper in evolution
```

在 Fedora 上安装邮件客户端：

```
sudo dnf in MozillaThunderbird 
```
```
sudo dnf in evolution
```

或者你可以使用浏览器使用在线的网页邮件客户端。

### 思维导图

开源的在线思维导图工具：[draw.io](https://app.diagrams.net/#)。draw.io 还支持 Linux 平台（有可用的 appimage 版本）。

### 字典

[Goldendict](http://goldendict.org/) 是一个功能强大，支持诸多字典格式的字典查词软件。

```
sudo zypper in goldendict goldendict-lang
sudo dnf in goldendict
```

其他：

- [FreeMdict Forum](https://forum.freemdict.com/) 是一个聚焦于各类字典与使用的爱好者论坛。
- [欧陆词典](https://dict.eudic.net/) 也是一个流行的字典软件，但 Linux 版不支持屏幕取词。
- [Bing 在线词典](https://www4.bing.com/dict?FORM=HDRSC6) 是一个简单易用的在线网络词典。
- [DeepL](https://www.deepl.com/translator)：免费的在线翻译引擎，由人工神经网络驱动。

### 电子书

Calibre 是一个功能丰富的开源图书管理软件。

```
sudo dnf in calibre
sudo zypper in calibre
```

通过 flatpak 安装：

```
flatpak install flathub com.calibre_ebook.calibre
```