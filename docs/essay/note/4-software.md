---
tags:
  - Linux
  - 笔记
---

# 软件包

## 代理软件

- [ ] [v2rayN]
- [ ] [nekoray]
- [x] [Clash-Verge-rev]
    - 依赖项：
        - `libayatana-appindicator3`
        - `libwebkit2gtk-4_1-0`

[v2rayN]: https://github.com/2dust/v2rayN/releases
[nekoray]: https://github.com/MatsuriDayo/nekoray/releases
[Clash-Verge-rev]: https://github.com/clash-verge-rev/clash-verge-rev/releases

steam 会自动读取系统代理设置。

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

### 安装 Flatpak 应用

- `com.dec05eba.gpu_screen_recorder`
- `com.github.dynobo.normcap`
- `com.github.johnfactotum.Foliate`
- `com.github.qarmin.czkawka`
- `com.github.tchx84.Flatseal`
- `com.jgraph.drawio.desktop`
- `io.github.xiaoyifang.goldendict_ng`
- `net.davidotek.pupgui2`
- `org.telegram.desktop`

### 清理数据

清理不再需要的库：

```
flatpak uninstall --unused
```

卸载时一并清理应用数据：

```
flatpak uninstall <package id> --delete-data
```

### czkawka

对比重复文件时，只需要添加一个文件夹就行了（不需要设置为参考文件夹）。

### fcitx5 皮肤没有正确显示（Wayland 会话）

**关闭所有 flatpak 应用**，然后运行：

```shell
flatpak override --env=QT_IM_MODULE= --user
flatpak override --env=GTK_IM_MODULE= --user
```

### 应用 GTK 主题与当前系统主题不匹配

使用 Flatseal 编辑应用，使其能访问主题文件夹（`/usr/share/themes`），然后为该应用设置如下环境变量：

```
GTK_THEME=Adwaita-dark
```

----

## 删除软件包

删除并锁定：

```
poplar@Greysia:~> zypper ll

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
10 | kuiviewer              | package | （任意）   | 
11 | opensuse-welcome       | package | （任意）   | 
12 | patterns-games-games   | package | （任意）   | 
13 | patterns-kde-kde_games | package | （任意）   | 
14 | patterns-kde-kde_pim   | package | （任意）   | 
15 | plymouth               | package | （任意）   | 
16 | skanlite               | package | （任意）   | 
17 | spectacle              | package | （任意）   | 
18 | vlc                    | package | （任意）   | 
```

快捷命令：

```
sudo zypper rm -u MozillaFirefox fcitx ibus kcalc kmousetool kompare konversation kuiviewer opensuse-welcome patterns-games-games patterns-kde-kde_games patterns-kde-kde_pim skanlite spectacle vlc
```
```
sudo zypper al MozillaFirefox fcitx ibus kcalc kmousetool kompare konversation kuiviewer opensuse-welcome patterns-games-games patterns-kde-kde_games patterns-kde-kde_pim skanlite spectacle vlc
```

- 另见[删除 plymouth](./10-plymouth.md)。

## 安装基本工具

- `chromium`
- `keepassxc`
- `proxychains-ng`
- `git-core`

快捷命令：

```
sudo zypper in chromium chromium-ffmpeg-extra chromium-plugin-widevinecdm keepassxc proxychains-ng git-core 
```

## 安装软件包

- `aria2`
- `audacious`
- `amdgpu_top`
- `fcitx5`
- `filelight`
- `flameshot`
- `google-noto-sans-mono-fonts`
- `gimp`
- `goverlay`
- `kfind`
- `kleopatra`
- `qbittorrent`
- `spek`
- `steam`

快捷命令：

```
sudo zypper in aria2 audacious amdgpu_top fcitx5 filelight flameshot google-noto-sans-mono-fonts gimp goverlay kfind kleopatra qbittorrent spek
```
```
sudo zypper in steam
```

### steam

如果无法正常缩放，则设置变量：

```
STEAM_FORCE_DESKTOPUI_SCALING=1.5
```

steam 会默认优先运行游戏的 Linux 原生版本。如果出现性能问题，请强制使用 steam proton 兼容工具。

如果存在连接性问题，则使用 `steam-proxy` 命令。

配置：

- 关闭着色器缓存
- 更改下载服务器
- 更改默认库位置
- 更改默认打开的页面
- 更改默认界面语言

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

注意，不要使用默认的存储池。关于与虚拟机共享文件，详见[此处]。

[此处]: ./../linux/1-kvm.md

### mpv

另见：<https://github.com/tomasklaen/uosc>

## Fcitx5

Fcitx5 的自定义词库文件（`*.dict`）：

- [felixonmars/fcitx5-pinyin-zhwiki]
- [wuhgit/CustomPinyinDictionary]

[felixonmars/fcitx5-pinyin-zhwiki]: https://github.com/felixonmars/fcitx5-pinyin-zhwiki
[wuhgit/CustomPinyinDictionary]: https://github.com/wuhgit/CustomPinyinDictionary

词库文件夹：`~/.local/share/fcitx5/pinyin/dictionaries/`

Fcitx5 皮肤：

```
git clone https://github.com/tonyfettes/fcitx5-nord.git
```

### flameshot

打开 KDE 的快捷键设置，添加 flameshot，然后将截图的快捷键绑定到 `PrtSc`。

----

## 散装软件包

可集成至开始菜单的 desktop 文件可通过 KDE 的右键新建菜单快速创建。

注意编辑环境变量时不要引入换行符。

### VSCodium

下载文件：<https://github.com/VSCodium/vscodium/releases>

在与 `VSCodium*.tar.gz` 同级文件夹中新建 `data/extensions` 和 `data/user-data`。

desktop 文件模板：

```shell
[Desktop Entry]
Categories=Development;
Comment[zh_CN]=
Comment=
Exec=/home/poplar/bin/codium/codium
GenericName[zh_CN]=
GenericName=
Icon=/home/poplar/bin/codium/data/paulo22s.png
MimeType=
Name[zh_CN]=VSCodium
Name=VSCodium
Path=/home/poplar/bin/codium
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

[shell-script]: ./8-shell-script.md

### pandoc

下载：<https://github.com/jgm/pandoc/releases/>

将下载好的 pandoc 解压至 `~/bin/pandoc`。符号连接可以放置到 `~/bin/command` 中，保持目录的整洁。

### FreeFileSync

下载：<https://freefilesync.org/download.php>

安装时，修改安装的用户范围（仅安装至当前用户）、路径（至 `~/bin/FreeFileSync`）。

然后安装缺失的依赖：

```
sudo zypper install libgthread-2_0-0
```

### mangohud & Goverlay

对于 steam 游戏，使用 `mangohud %command%`。使用说明详见：`man mangohud`

注意：

1. 不要在游戏启动时让 mangohud 读取游戏的平均帧和 1% low 帧
1. 使用 Goverlay 时，如果有按钮被遮挡，可以把显示器缩放比例从 150% 调整为 100%