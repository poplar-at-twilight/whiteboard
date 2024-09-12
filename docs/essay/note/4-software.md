---
comments: true
tags:
  - Linux
  - 笔记
---

# 软件包

## clash-verge-rev

下载：

- <https://github.com/Clash-Verge-rev/clash-verge-rev/releases>

安装依赖：

```
sudo zypper in libayatana-appindicator3-1 libwebkit2gtk-4_0-37
```

安装本体：

```
sudo rpm -i clash-verge-*.x86_64.rpm
```

## Flatpak

安装：

```
sudo zypper in flatpak
```

添加远程仓库：

```
set-proxy; flatpak remote-add --if-not-exists --user flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

加入 Flatpak 用户组

```
sudo usermod -aG flatpak $USER
```

### 安装

```
flatpakx install com.calibre_ebook.calibre
flatpakx install com.jgraph.drawio.desktop
flatpakx install com.github.qarmin.czkawka
flatpakx install com.usebottles.bottles
flatpakx install org.telegram.desktop
flatpakx install org.freefilesync.FreeFileSync
```

### 清理不再需要的库

```
flatpak uninstall --unused
```

### Bottles

创建容器后，需要安装一些前置依赖：

- 系统字体包
- CJK 字体包
- VC++ 2015-2022

Bottles 的 desktop 文件的环境变量可设置为：

```
https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890
```

### fcitx5 皮肤没有正确显示（Wayland 会话）

关闭所有 flatpak 应用，然后运行：

```shell
flatpak override --env=QT_IM_MODULE= --user
flatpak override --env=GTK_IM_MODULE= --user
```

----

## 删除软件包

删除并锁定：

```
poplar@c004-h1:~> zypper ll

#  | Name                   | Type    | Repository | Comment
---+------------------------+---------+------------+--------
1  | MozillaFirefox         | package | （任意）   | 
2  | PackageKit             | package | （任意）   | 
3  | discover6              | package | （任意）   | 
4  | fcitx                  | package | （任意）   | 
5  | ibus                   | package | （任意）   | 
6  | kcalc                  | package | （任意）   | 
7  | kmousetool             | package | （任意）   | 
8  | kompare                | package | （任意）   | 
9  | konversation           | package | （任意）   | 
10 | opensuse-welcome       | package | （任意）   | 
11 | patterns-games-games   | package | （任意）   | 
12 | patterns-kde-kde_games | package | （任意）   | 
13 | patterns-kde-kde_pim   | package | （任意）   | 
14 | skanlite               | package | （任意）   | 
```

快捷命令：

```
sudo zypper rm -u MozillaFirefox PackageKit discover6 fcitx ibus kcalc kmousetool kompare konversation opensuse-welcome patterns-games-games patterns-kde-kde_games patterns-kde-kde_pim skanlite
```
```
sudo zypper al MozillaFirefox PackageKit discover6 fcitx ibus kcalc kmousetool kompare konversation opensuse-welcome patterns-games-games patterns-kde-kde_games patterns-kde-kde_pim skanlite
```

## 安装软件包

- `aria2`
- `audacious`
- `crow-translate`
- `fcitx5`
- `filelight`
- `FreeFileSync`
- `google-noto-sans-mono-fonts`
- `gimp`
- `goldendict-ng`
  - `goldendict-ng-lang`
- `kfind`
- `kleopatra`
- `steam`

快捷命令：

```
sudo zypper in aria2 audacious crow-translate crow-translate-lang fcitx5 filelight FreeFileSync google-noto-sans-mono-fonts gimp goldendict-ng goldendict-ng-lang kfind kleopatra  
```
```
sudo zypper in steam
```

### CrowTranslate

从 [Traineddata Files for Version 4.00 +](https://tesseract-ocr.github.io/tessdoc/Data-Files.html) 下载所需文件。

- chi_sim.traineddata
- chi_sim_vert.traineddata
- chi_tra.traineddata
- chi_tra_vert.traineddata
- eng.traineddata

默认关闭 chi_tra 文件。

### steam

如果无法正常缩放，则设置变量：

```
STEAM_FORCE_DESKTOPUI_SCALING=1.5
```

steam 会默认优先运行游戏的 Linux 原生版本。如果出现性能问题，请强制使用 steam proton 兼容工具。

如果存在连接性问题，则使用 `steam-proxy` 命令。

### KVM

安装软件包：

```
sudo zypper install -t pattern kvm_server kvm_tools
```

加入用户组：

```
sudo usermod -aG libvirt $USER
```

注销重新登录。

注意，不要使用默认的存储池。

关于与虚拟机共享文件，详见[此处]。

[此处]: https://zh.opensuse.org/KVM#.E4.B8.8E.E8.99.9A.E6.8B.9F.E6.9C.BA.E5.85.B1.E4.BA.AB.E6.96.87.E4.BB.B6

## 散装软件包

- <https://github.com/c0re100/qBittorrent-Enhanced-Edition/releases>
- <https://github.com/yang991178/fluent-reader/releases>

### vscode

下载：

- <https://code.visualstudio.com/>

在与 `code` 同级文件夹中新建 `data`，启用便携模式。

字体设置：

```
'Noto Sans Mono', 'Noto Sans SC', monospace
```