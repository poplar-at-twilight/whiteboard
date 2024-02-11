---
comments: true
tags:
  - 新闻
  - xfce
  - Wayland
---

# Xfce4 Wayland 开发路线图

- 译文信息：
    - 原文：[Xfce Wayland Development Roadmap](https://wiki.xfce.org/releng/wayland_roadmap)
    - 类型：维基词条
    - 许可证：[CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en)
    - 日期：2024-02-11
    - 译者：暮光的白杨

----

!!! warning "注意"

    - 本文档旨在供 [Xfce] 开发人员跟踪 [Wayland] 开发。这是一项正在进行的工作，并不意味着任何未来的实施承诺。
    - 本文只是原文档于 2024 年 2 月 11 日的版本的译文。

## 总体规划

对于 Xfce 4.20，我们的计划是在不失去 [X11] 支持的前提下，为核心组件添加对 Wayland 的初步支持。这并不意味着在下一个主要版本中，Xfce 的 Wayland 会话将提供所有现有功能，但我们希望它能在最低程度上可用。我们还打算继续完善我们（那些已经可以运行或只需少量工作即可运行的应用程序）的应用程序，使其能在 Wayland 上正常运行。

[X11]: https://x.org/wiki/
[Xfce]: https://xfce.org/
[wayland]: https://wayland.freedesktop.org/

查看[组件特定状态](#_3)部分的表格，了解详细信息和按组别标注的问题：

- [核心组件](#_4)
- [应用](#_5)
- [Thunar 插件](#thunar)
- [面板插件](#_6)

## 长期目标

目前还不清楚哪个 Xfce 版本将实现 Xfce Wayland 的完全过渡（或是否会实现过渡）。下面列出了为实现过渡而需要完成的大型任务。

其中一些大部分得到了 Xfce 开发人员的认可。

**商定的指导方针**

- 不依赖 [XWayland]
    - 没有 [xsettings]
- 使用 [wlroots] 而不是 [libmutter]
    - 保留将 xfdesktop 和 xfce4-panel 作为单独组件运行的可能性
    - 防止对 [libgnome-desktop] 的依赖
    - xfce4-panel 和 xfdesktop 已移植到 Wayland，假设我们的混成器将基于 wlroots。
    - [xfwm4 的非官方移植]也正在进行中。
- 在可预见的未来保持 X11 兼容性
    - Nvidia 对 Wayland 的支持仍然存在问题，这是保持 X11 向后兼容性的又一个原因（[nouveau] 驱动程序通常性能孱弱）
    - 像 [Weston] 或 [sway] 这样从头开始编写的 Wayland 混成器永远不会作为 X11 窗口管理器运行。但其他以 X11 窗口管理器（例如 kwin 或 mutter）开始的仍然保留他们的 x11 窗口管理代码
    - [FreeBSD] 对 Wayland 提供了不错的[支持]，[OpenBSD] [仍在努力支持] Wayland

[xfwm4 的非官方移植]: https://github.com/adlocode/xfwm4/tree/wayland
[XWayland]: https://wayland.freedesktop.org/xserver.html
[xsettings]: https://wiki.archlinux.org/title/Xsettingsd
[wlroots]: https://gitlab.freedesktop.org/wlroots
[libmutter]: https://mutter.gnome.org/
[libgnome-desktop]: https://github.com/GNOME/gnome-desktop
[weston]: https://wayland.pages.freedesktop.org/weston/
[sway]: https://swaywm.org/
[支持]: https://docs.freebsd.org/en/books/handbook/wayland/
[仍在努力支持]: https://www.openbsd.org/papers/eurobsdcon2023-matthieu-wayland-openbsd.pdf
[FreeBSD]: https://www.freebsd.org/
[OpenBSD]: https://www.openbsd.org/
[nouveau]: https://nouveau.freedesktop.org/

## 组件特定状态

### 核心组件

此表反映了 Xfce 4.19 或 git master 发布的内容的当前状态。

|组件名|Wayland</br>支持|备注|
|:---|:---:|:---|
|exo|✅||
|libxfce4ui|✅|[libxfce4ui!110] 中的 X11 代码路径得到正确保护|
|libxfce4util|✅||
|thunar|✅||
|xfce4-appfinder|✅||
|[xfce4-panel]|✅|见下文|
|xfce4-session|❌||
|[xfce4-settings]|✅|见下文|
|xfconf|✅||
|[xfdesktop]|✅|见下文|
|[xfwm4]|❌||
|[xfce4-power-manager]|✅|见下文|
|tumbler|✅||
|garcon|✅||
|thunar-volman|✅||
|xfce4-dev-tools|✅||

[libxfce4ui!110]: https://gitlab.xfce.org/xfce/libxfce4ui/-/merge_requests/110
[xfce4-panel]: https://wiki.xfce.org/releng/wayland_roadmap#xfce4-panel
[xfce4-settings]: https://wiki.xfce.org/releng/wayland_roadmap#xfce4-settings
[xfdesktop]: https://wiki.xfce.org/releng/wayland_roadmap#xfdesktop
[xfwm4]: https://wiki.xfce.org/releng/wayland_roadmap#xfwm4
[xfce4-power-manager]: https://wiki.xfce.org/releng/wayland_roadmap#xfce4-power-manager

#### xfce4-panel

- Wayland 移植工作已完成：[xfce4-panel!103]
    - 基于 Wlroots 的目标混成器：[Labwc]、[Wayfire]
- 在 Wayland 上，面板无法再使用 GtkSocket/GtkPlug 将插件作为外部（单独的进程）运行。最初，为了推进其他功能的移植，将它们作为内部运行就足够了（与面板相同的进程，因此插件崩溃会导致面板崩溃）。如果我们想之后“原生地”恢复它，似乎我们必须在某种程度上使面板成为 Wayland 混成器（[嵌入混成器]，另请参阅[允许嵌入外部 wl_surfaces]）。目前，socket/插件结构已使用 [layer-shell 协议]和 [D-Bus] 在 Wayland 上重现，虽然不是原生的，但具有简单性和重用现有内容的优点（更多详细信息请参见[此处]）。

[xfce4-panel!103]: https://gitlab.xfce.org/xfce/xfce4-panel/-/merge_requests/103
[labwc]: https://github.com/labwc/labwc
[wayfire]: https://github.com/WayfireWM/wayfire
[嵌入混成器]: https://wayland.freedesktop.org/docs/html/ch02.html#sect-Compositors-Embedding-Compositor
[允许嵌入外部 wl_surfaces]: https://gitlab.freedesktop.org/wayland/wayland-protocols/-/issues/74
[layer-shell 协议]: https://wayland.app/protocols/wlr-layer-shell-unstable-v1
[D-Bus]: https://www.freedesktop.org/wiki/Software/dbus/
[此处]: https://mail.xfce.org/pipermail/xfce4-dev/2022-October/033092.html

#### xfdesktop

- Wayland 移植工作已完成：[xfdesktop!43]
- 工作区支持需要 X11/Wayland 抽象（abstraction），并且可以在 Wayland 上使用 `wlr-workspace-unstable-v1` 协议。
- 列出所有顶层窗口（窗口列表菜单、桌面上的窗口图标）需要 X11/Wayland 抽象，并且可以在 Wayland 上使用 `wlr-foreign-toplevel-management-unstable-v1` 协议。

[xfdesktop!43]: https://gitlab.xfce.org/xfce/xfdesktop/-/merge_requests/43

#### xfwm4

- 非官方的 Wayland 移植正在进行中：<https://github.com/adlocode/xfwm4/tree/wayland>

#### xfce4-settings

- [xfce4-settings!104] 中添加了 Wayland 上 xsettings 的对应项，只需进行最少的修改即可使 xfsettingd 在 Wayland 上可执行。
- [xfce4-settings!112] 中已将显示设置移植到 Wayland，这次在整个 xfce4-settings 中完全分离了 X11/Wayland 代码路径。
- 在 X11 特有的实现中，只有显示设置适合移植到 Wayland。目前还没有其余的协议（键盘、鼠标、工作区等，都是混成器内部的）。

[xfce4-settings!104]: https://gitlab.xfce.org/xfce/xfce4-settings/-/merge_requests/104
[xfce4-settings!112]: https://gitlab.xfce.org/xfce/xfce4-settings/-/merge_requests/104

#### xfce4-power-manager

- Wayland 移植工作已完成：[xfce4-power-manager!54]
- 用户不活动监控和显示电源管理的基本功能已通过 [ext-idle-notify] 协议和 [wlr-output-power-management] 协议恢复。
- 亮度管理没有真正的对应项（[wlr-gamma-control] 协议在外观上提供类似的功能，但实际上与节能无关），但现有的 [Polkit] 实现独立于窗口环境工作。
- 与键盘处理（亮度键、暂停键等）有关的所有内容都不会恢复，因为当前只有混成器能够管理它。

[xfce4-power-manager!54]: https://gitlab.xfce.org/xfce/xfce4-power-manager/-/merge_requests/54
[ext-idle-notify]: https://wayland.app/protocols/ext-idle-notify-v1
[wlr-output-power-management]: https://wayland.app/protocols/wlr-output-power-management-unstable-v1
[wlr-gamma-control]: https://wayland.app/protocols/wlr-gamma-control-unstable-v1
[Polkit]: https://wiki.archlinux.org/title/Polkit

### 应用

|组件名|Wayland</br>支持|备注|
|:---|:---:|:---|
|xfce4-terminal|✅|下拉需要 layer-shell 才能正常工作</br>[xfce4-terminal!72]|
|mousepad|✅||
|[xfce4-notifyd]|✅||
|xfdashboard|❌|启动时崩溃|
|xfce4-taskmanager|✅|没有 libwnck（appicons、[xfce4-taskmanager#75]）、没有“识别窗口”、没有系统托盘图标（[xfce4-taskmanager#78]）|
|xfce4-mixer|✅||
|ristretto|✅||
|catfish|✅||
|xfburn|✅||
|parole|✅|没有系统托盘图标（[parole#126]）|
|[xfce4-screenshooter]|❌|正在处理中 [xfce4-screenshooter!52]，见下文|
|xfce4-screensaver|❌|移植到 Wayland 基本上是通过 [xfce4-screensaver!28] 完成的</br>但这需要 `libwlembed`，它仍处于实验阶段，现阶段尚未发布 (2024-02-04)|
|xfmpc|✅||
|xfce4-volumed-pulse|❌|按键绑定失败|
|xfce4-dict|✅||
|gigolo|✅||
|xfce4-panel-profiles|✅||

[xfce4-terminal!72]: https://gitlab.xfce.org/apps/xfce4-terminal/-/merge_requests/72
[xfce4-notifyd]: https://wiki.xfce.org/releng/wayland_roadmap#xfce4-notifyd
[xfce4-screenshooter]: https://wiki.xfce.org/releng/wayland_roadmap#xfce4-screenshooter
[xfce4-taskmanager#75]: https://gitlab.xfce.org/apps/xfce4-taskmanager/-/issues/75
[xfce4-taskmanager#78]: https://gitlab.xfce.org/apps/xfce4-taskmanager/-/issues/78
[parole#126]: https://gitlab.xfce.org/apps/parole/-/issues/126
[xfce4-screenshooter!52]: https://gitlab.xfce.org/apps/xfce4-screenshooter/-/merge_requests/52
[xfce4-screensaver!28]: https://gitlab.xfce.org/apps/xfce4-screensaver/-/merge_requests/28

#### xfce4-notifyd

如果你将窗口设置为覆盖重定向或始终在顶部，GTK 似乎不会执行任何特殊操作。

#### xfce4-screenshooter

Wayland 没有为混成器指定用于截取屏幕截图的原生接口，而且[看起来永远不会]。

因此对于 xfce4-screenshooter 有以下选项：

- 添加对 wlroots screencopy 协议的支持
    - [wlr-screencopy-unstable-v1.xml]
    - 我们的混成器很可能支持这个协议
- 如果我们想要支持未实现上述协议的混成器，我们必须为 [org.freedesktop.portal.Screenshot] 添加 DBus 支持，即：
    - xdg-desktop-portal-gnome
    - xdg-desktop-portal-kde

[看起来永远不会]: https://gitlab.freedesktop.org/wayland/wayland/-/issues/32
[wlr-screencopy-unstable-v1.xml]: https://gitlab.freedesktop.org/wlroots/wlr-protocols/-/blob/master/unstable/wlr-screencopy-unstable-v1.xml
[org.freedesktop.portal.Screenshot]: https://github.com/flatpak/xdg-desktop-portal
[xdg-desktop-portal-gnome]: https://gitlab.gnome.org/GNOME/xdg-desktop-portal-gnome
[xdg-desktop-portal-kde]: https://github.com/KDE/xdg-desktop-portal-kde

### Thunar 插件

|组件名|Wayland</br>支持|备注|
|:---|:---:|:---|
|thunar-archive-plugin|✅||
|thunar-media-plugin|✅||
|thunar-vcs-plugin|✅||
|thunar-shares-plugin|✅||

### 面板插件

有关如何在 Wayland 上运行外部插件的详细信息，请参阅 [xfce4-panel](#xfce4-panel) 部分。首先，下面的 “✅” 仅仅意味着“不会崩溃”，即使经过一些基本操作（最终）也是如此。这并不意味着一切都像 X11 上那样工作。

以下测试是在 2022 年 10 月 12 日通过 git-master 为每个插件构建进行的。

|组件名|Wayland</br>支持|备注|
|:---|:---:|:---|
|xfce4-battery-plugin|✅||
|xfce4-calculator-plugin|✅|对输入文本无反应|
|xfce4-clipman-plugin|✅|通过 wlr-data-control [移植]</br>TODO：[使用状态通知器而不是状态图标]|
|xfce4-cpufreq-plugin|✅||
|xfce4-cpugraph-plugin|✅||
|xfce4-datetime-plugin|✅||
|xfce4-diskperf-plugin|✅||
|xfce4-docklike-plugin|❌|崩溃（[Libwnck]）|
|xfce4-embed-plugin|❌|未维护，gtk2|
|xfce4-eyes-plugin|✅|面板外不跟随指针|
|xfce4-fsguard-plugin|✅||
|xfce4-generic-plugin|✅||
|xfce4-genmon-plugin|✅||
|xfce4-indicator-plugin|✅||
|xfce4-mailwatch-plugin|✅||
|xfce4-mount-plugin|✅||
|xfce4-mpc-plugin|✅|虽然可能依赖于不工作的东西（？）|
|xfce4-netload-plugin|✅||
|xfce4-notes-plugin|✅||
|xfce4-places-plugin|✅|图标问题，删除插件时出现严重错误|
|xfce4-pulseaudio-plugin|✅|有警告（需要重新检查）|
|xfce4-sample-plugin|✅||
|xfce4-sensors-plugin|✅||
|xfce4-smartbookmark-plugin|✅|对输入文本无反应|
|xfce4-statusnotifier-plugin|❌|崩溃（gdk_x11 代码，从 4.15.4 开始合并到系统托盘插件中）|
|xfce4-stopwatch-plugin|✅||
|xfce4-systemload-plugin|✅||
|xfce4-time-out-plugin|✅|删除/重新添加插件时卡住和严重错误时的核心转储|
|xfce4-timer-plugin|✅||
|xfce4-verve-plugin|✅|对输入文本无反应|
|xfce4-wavelan-plugin|✅||
|xfce4-weather-plugin|✅||
|xfce4-whiskermenu-plugin|✅|图标问题，菜单窗口浮动|
|xfce4-windowck-plugin|❌|无法工作（Libwnck）|
|xfce4-xkb-plugin|❌|无法工作（Libwnck）|

[移植]: https://gitlab.xfce.org/panel-plugins/xfce4-clipman-plugin/-/merge_requests/26
[使用状态通知器而不是状态图标]: https://gitlab.xfce.org/panel-plugins/xfce4-clipman-plugin/-/issues/87
[libwnck]: https://developer-old.gnome.org/libwnck/stable/

## 测试

有关测试特定组件的信息。

关于要测试的版本：最好是 master 版本或最新的开发版本，当然最新的稳定版本也可以。目前大多数组件的版本差别不大。如果你没有测试 master 版本，最好添加信息说明你测试的版本。

如果你使用的是英伟达 GPU，则需要使用 "Nouveau" 驱动程序进行测试，因为英伟达的专有驱动程序不支持 Wayland。（尽管在某些情况下有些东西可能会工作）。

### Wayland 原生

- 使用包管理器安装 `Weston`（如果需要基于 Wlroots 的混成器，如 [xfce4-panel]，请参阅[此 MR] 以获取更多信息）
- 也许你需要设置一个最小配置，例如：
    ```
    ~/.config/weston.ini

    [keyboard]
    keymap_layout=fr
    numlock-on=true
    ```
- 在终端模拟器中运行 `weston`，或者最好在不同用户的 `tty` 中运行，或者先注销 X11 会话（混成器的行为可能不太一样，这样可以避免与当前环境发生交互）。
- 如果在终端模拟器中运行，请确保至少要测试的组件尚未在 X11 会话中运行（例如 Thunar 作为后台程序）
- 在 `Weston` 会话中打开终端并启动要测试的组件

[此 MR]: https://gitlab.xfce.org/xfce/xfce4-panel/-/merge_requests/103

### 外部链接

- [将 MATE 桌面环境的应用程序移植到 wayland](https://discourse.ubuntu.com/t/porting-mate-apps-to-wayland/12670)
- [深入探讨 Wayland 协议](https://frontpagelinux.com/articles/a-deep-dive-into-the-wayland-protocol-for-linux)
- [检查当前显示是否为 Wayland 显示](https://gitlab.xfce.org/xfce/libxfce4ui/-/commit/57410e2dffde5271443809a0dbd0280be262ba47)

