---
comments: true
tags:
  - 新闻
  - TheEvilSkeleton
  - Flatpak
---

# 不推荐 Flatpak 用户做的事情

## 作品信息

- 原文：[What Not to Recommend to Flatpak Users](https://theevilskeleton.gitlab.io/2022/09/28/what-not-to-recommend-to-flatpak-users.html)
- 作者：[TheEvilSkeleton](https://theevilskeleton.gitlab.io/about)
- 许可证：[CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/legalcode)
- 译者：暮光的白杨
- 日期：2022-10-18

----

## 简介

每当我浏览网络时，我都会从各种博客作者、YouTuber 和其他建议用户采取他们不应该采取的步骤或有更好的选择的其他人那里找到许多“提示和技巧”。在本文中，我将介绍一些你不应该采取的步骤并解释原因。

## 设置 GTK_THEME

`GTK_THEME` 变量通常用于强制 GTK 应用程序使用自定义主题。例如，设置 `GTK_THEME=adw-gtk3-dark` 将设置 [adw-gtk3](https://github.com/lassekongo83/adw-gtk3) 的深色变体（如果有安装到 flatpak 上）作为应用主题。

`GTK_THEME` 是一个[调试变量](https://docs.gtk.org/gtk4/running.html#gtk_theme)。它旨在用于测试 GTK3 和 GTK4 的样式表。但是，它**不打算**供用户使用。libhandy 和 libadwaita 提供了大多数 GTK3 和 GTK4 主题不支持的附加小部件，因为它们是为 GTK3 或 GTK4 单独制作的。这意味着在 GTK4 + libadwaita 应用程序上使用自定义主题可能会删除 libadwaita 小部件。

许多应用程序正逐渐从 GTK3 移植到 GTK4 + libadwaita。虽然 `GTK_THEME` 在 GTK3 上可以正常工作，但如果保留 `GTK_THEME` 变量，应用程序在移植到 GTK4 + libadwaita 后会出现[故障](https://github.com/bottlesdevs/Bottles/issues/1663)。这种情况下的解决方案是取消设置 `GTK_THEME`。

推荐 `GTK_THEME` 时，请确保用户知道在应用程序移植到 GTK4 + libadwaita 后他们需要取消设置该变量。或者更好的办法是，不要向用户推荐调试变量。否则，他们会觉得应用程序本身有问题并且无法按预期工作。他们不会知道问题是 `GTK_THEME` 引起的。

## 为 flatpak run 设置别名

一个常见的建议是给 `flatpak run` 设置别名。从终端启动 Flatpak 应用程序时，通常键入 `flatpak run $APP_ID`，其中 `$APP_ID` 是应用程序 ID，例如 `org.mozilla.firefox` 对应 Firefox。因此，从逻辑上讲，用户发现 `flatpak run` 命令太长，因此他们设置了别名，例如 `fr`，`alias fr="flatpak run org.mozilla.firefox"`。

虽然这可行，但有更好的方法来改善这种情况。 Flatpak 有自己的 `/bin` 目录，我们可以将其添加到 `PATH` 中。 对于系统安装[^1]，该目录位于 `/var/lib/flatpak/exports/bin`。对于用户安装[^1]，它位于 `~/.local/share/flatpak/exports/bin`。

重新启动 shell 后，我们应该可以只使用应用程序 ID 启动程序，而无需键入 `flatpak run`。默认情况下，这些目录未设置为 `PATH`，以避免 Flatpak 的 `/bin` 与遵循[反向域名表示法](https://en.wikipedia.org/wiki/Reverse_domain_name_notation)约定的发行版软件包的二进制文件发生冲突。

`flatpak run` 通常用于在运行 Flatpak 应用程序时临时添加或删除权限。例如， `flatpak run --nofilesystem=home $APP_ID` 专门针对该会话拒绝 `$APP_ID` 访问 `filesystem=home`。

## 将主题和图标放置在 ~/.icons 和 ~/.themes

用户经常犯的一个主要错误是覆盖文件系统权限以允许对 `~/.icons` 和 `~/.themes` 进行读取访问。Flatpak 完全不支持上述路径，因为它们是旧目录。

使用 `~/.icons` 中的光标的用户可能会遇到 Flatpak 应用程序回退到 [Xlib 光标](https://www.oreilly.com/library/view/xlib-reference-manual/9780937175262/images/p099-001.jpg)的问题。使用 `~/.themes` 的用户可能会遇到 Flatpak 无法自动检测主题并安装它（如果可用）的问题。

Flatpak 严重依赖 XDG 标准，因此遵循 XDG 兼容的等效路径：

- `~/.icons` → `~/.local/share/icons`
- `~/.themes` → `~/.local/share/themes`

最好使用这些符合 XDG 的路径来避免覆盖权限，因为它们得到更好的长期支持。如果你使用在旧路径中安装光标/图标或主题的程序，请联系开发人员并请他们遵循 XDG 标准！

[^1]: https://docs.flatpak.org/en/latest/using-flatpak.html#system-versus-user