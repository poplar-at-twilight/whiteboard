---
comments: true
tags:
  - 新闻
  - Fedora
  - GNOME
---

# Fedora Workstation 40 的新亮点

- 译文信息：
    - 原文：[What’s new in Fedora Workstation 40](https://fedoramagazine.org/whats-new-fedora-workstation-40/)
    - 作者：[Roseline Bassey](https://fedoramagazine.org/author/roseline-bassey/)
    - 许可证：[CC-BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/)
    - 日期：2024-04-24
    - 译者：暮光的白杨

----

> ![](./images/2024-04/fedora/F40-image-1024x433.jpg)
> 图片由 Consolation Obazee 提供

随着 Fedora Workstation 40 的发布，Fedora 项目的旗舰开源 Linux 桌面操作系统 Fedora Workstation 达到了一个新的里程碑。这个版本的发布得益于我们全球社区的贡献，包括您的贡献！Fedora Workstation 40 拥有众多新特性和性能增强，为您带来更流畅、响应更迅速的计算体验。请继续阅读以下部分，了解最新功能和改进。您可以从 [Fedora Workstation 首页]下载 Fedora Workstation 40，或在 Software 应用程序中升级已安装的系统，或在您最喜欢的终端模拟器中使用 [*dnf system-upgrade*] 进行升级。

[Fedora Workstation 首页]: https://fedoraproject.org/workstation/
[*dnf system-upgrade*]: https://docs.fedoraproject.org/en-US/quick-docs/upgrading-fedora-offline/

## GNOME 46

Fedora Workstation 40 采用了最新版本的 GNOME 桌面环境，GNOME 46。主要更新包括[文件]应用程序的显著升级，引入了新功能和增强功能。此外，可访问性的许多方面也得到了改进，确保了更具包容性的用户体验。[设置]应用程序和其他核心应用程序也得到了改进，以提高可用性。更多详情请查看 [GNOME 46 发行说明]。

[GNOME 46 发行说明]: https://release.gnome.org/46/

[设置]: https://apps.gnome.org/zh-CN/Settings/
[软件]: https://apps.gnome.org/zh-CN/Software/
[文件]: https://apps.gnome.org/zh-CN/Nautilus/
[Flatpak]: https://www.flatpak.org/
[日历]: https://apps.gnome.org/zh-CN/Calendar/
[连接]: https://apps.gnome.org/zh-CN/Connections/
[RDP]: https://en.wikipedia.org/wiki/Remote_Desktop_Protocol

GNOME 46 还进行了许多其他改进，例如：

- 按应用程序对通知进行分组。现在，每个通知都有一个标题。标题会显示应用程序的名称和图标。这样就可以查看哪个应用程序发送了消息。通知现在还有一个展开按钮。
- 现在，您可以通过添加 Ctrl 修饰键为固定到仪表板上的应用程序打开一个新窗口。例如：`Super + Ctrl + 1` 为仪表板中的第一个应用程序打开一个新窗口，与现有的快捷键 `Super + <Number>` 相辅相成，后者可启动应用程序本身。
- 触摸板现在已默认启用 "轻点即点击（Tap to Click）" 功能。
- GNOME 46 现在具有远程登录选项。当没有活动会话时，您可以使用 RDP 远程连接到一个新的专用桌面会话。

## 核心应用

GNOME 的核心应用程序在新版本中有了显著的改进。其中包括：

### 设置

GNOME 46 对[设置]应用程序进行了令人兴奋的更新，使其比以往任何时候都更加用户友好。最新版本增加了更多键盘助记符，使导航更容易。它还拥有时尚现代的界面。外观设置的加载速度比以前更快，预览也更清晰。此新版本提供了对 Wacom 手写笔压力的更精确控制。

除了上述升级以外，设置应用还有一些值得一提的重大改进：

- 设置应用新增了一个系统面板。它将**地区和语言**、**日期和时间**、**远程桌面**和**关于**组合到一个设置面板中。这种新设计使应用更易于浏览。
- GNOME 46 更新了触摸板设置，增加了两个新选项。第一个选项名为 "辅助单击（Secondary Click）"，可让您选择如何在触摸板上执行右键单击：用两根手指或单击某个角落。第二个选项允许您在键入时保持触摸板处于活动状态，这在某些需要同时使用键盘和触摸板的应用程序和游戏中很有帮助。

### 文件

- [文件]应用的显著升级之一是引入了新的全局搜索功能。全局搜索功能允许您在所有配置的位置搜索文件。您可以搜索文件内容，按类型和修改日期过滤文件，还可以同时搜索多个位置。单击文件路径字段旁边的图标以激活此功能。
- 在 GNOME 46 中，底部的侧边栏动态进度部分可让您更有效地监控文件操作，并提供更多有关其进度的详细信息。
- 现在可以快速切换文件应用的列表视图和网格视图。这解决了以前版本中出现的滞后问题。

文件应用的其他更改包括在偏好设置中增加了一个新的搜索字段；它有助于查找特定设置。现在还可以选择以一致的格式显示日期和时间；同时文件改进了网络发现功能。这些改进让文件管理更高效。

### 其他核心应用程序也得到了升级

- [软件]应用程序现在会显示受信任的 Flathub 应用程序的验证徽章，确保软件的真实性。
- [地图]应用提供全新的编辑体验，支持深色模式，并扩展了公共交通路线。

[地图]: https://apps.gnome.org/Maps/

- [扩展程序]和[日历]应用获得现代化的设计和可用性的改进。

[扩展程序]: https://apps.gnome.org/zh-CN/Extensions/

- GNOME 46 升级了[时钟]和[通讯录]应用程序。您现在可在时钟中快速设置计时器。此外，您还能在通讯录中一次性导入多个 VCard 文件。

[时钟]: https://apps.gnome.org/zh-CN/Clocks/
[通讯录]: https://apps.gnome.org/zh-CN/Contacts/

- [磁盘]应用新增了 I/O 资源图表，用于监控磁盘使用情况。

[磁盘]: https://apps.gnome.org/zh-CN/DiskUtility/

## 性能改进

GNOME 46 提供了实质性的底层改进，以提供更高效、更完美的体验。主要改进包括：

- 减少搜索时的内存使用量。
- 大幅提升[终端]应用程序的运行速度。

[终端]: https://apps.gnome.org/zh-CN/Console/

- 更吸引人的视觉效果，因为 GTK 的新渲染技术让应用程序界面更清晰，屏幕上的文字更清晰，用户界面元素更明确，尤其是在使用分数显示比例时。
- 试验性支持[可变刷新率]（VRR），实现更流畅的视频性能。您可以使用命令启用该功能：<br /><em>
`gsettings set org.gnome.mutter experimental-features "['variable-refresh-rate']"`
</em><br />
启用后，可在显示设置中设置刷新率。

[可变刷新率]: https://en.wikipedia.org/wiki/Variable_refresh_rate

## Fedora Linux 40 的底层改进

Fedora Linux 40 有许多底层变化。下面是一些值得注意的改进：

- NetworkManager 默认启用 IPV4 地址冲突检测，以解决同一物理网络中重复 IPV4 地址造成的冲突。
- Fedora 40 将 PyTorch 直接集成到其软件仓库中。这样，用户就能更轻松地为自己的项目访问开源机器学习框架。现在只需一条命令就能轻松完成安装：<br /><em><code>
sudo dnf install python3-torch
</code></em>
- 从 Fedora Linux 40 开始，"不可变（immutable）"一词将不再用于描述所有基于 [rpm-ostree] 的 Fedora Linux 变种（Silverblue、Kinoite、Sway 和 Budgie）。相反，它们将被称为 "原子化（Atomic）"桌面，例如 Sericea 现在被称为 Fedora Atomic Sway。这一变化是品牌重塑的一部分，旨在简化 Fedora Spins 的命名规则。有关这一变更的更多信息，请点击[此处]。

[rpm-ostree]: https://ostreedev.github.io/ostree/
[此处]: ./fedora-atomic-de-intro.md

## 也请查看……

也请看看 Fedora 项目中发生的有趣事件！

敬请期待，并准备好与 Fedora 社区一起参与即将到来的活动！请与我们一起在 6 月前往[捷克布尔诺]参加 [DevConf CZ 会议]——这是一次充满深刻讨论和研讨会的聚会，您将有机会结识其他爱好者。

[捷克布尔诺]: https://en.wikipedia.org/wiki/Brno
[DevConf CZ 会议]: http://www.devconf.info/cz/

然后，请记下八月份的日历，届时我们的旗舰贡献者会议 Flock 将举行。有关 Flock 2024 的更多详情，请查看[这篇文章]。

[这篇文章]: ./fedora-flock-2024.md