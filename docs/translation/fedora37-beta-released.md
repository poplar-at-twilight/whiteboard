---
comments: true
tags:
  - 新闻
  - Fedora
  - GNOME
---

# Fedora Linux 37 Beta 版本现已发布

## 作品信息

- 原文：[Announcing the release of Fedora Linux 37 Beta](https://fedoramagazine.org/announcing-fedora-37-beta/)
- 作者：[Matthew Miller](https://fedoramagazine.org/author/mattdm/)
- 日期：2022-09-14
- 译者：暮光的白杨
- 许可证：[CC-BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/)

---

![image](./images/2022-09/f37-beta-1024x433.jpg)

Fedora 项目很高兴地宣布 Fedora Linux 37 Beta 现已发布，这是我们计划在 10 月底发布 Fedora Linux 37 的下一步。

你可从我们的 Get Fedora 网站下载预发布版本：

- [获取 Fedora 37 Workstation Beta](https://getfedora.org/workstation/download/)
- [获取 Fedora 37 Server Beta](https://getfedora.org/server/download/)
- [获取 Fedora 37 IoT Beta](https://getfedora.org/iot/download/)

或者你可以浏览流行的 Fedora 变种版本（[Fedora Spin](https://spins.fedoraproject.org/)），这其中包括 KDE Plasma、Xfce 和其他桌面环境，以及用于特定用例（如计算神经科学）的 Fedora Labs。

- [获取 Fedora Spins 37 Beta](https://spins.fedoraproject.org/prerelease)
- [获取 Fedora Labs 37 Beta](https://labs.fedoraproject.org/prerelease)

## Beta 版本亮点

### Fedora Workstation

Fedora 37 Workstation Beta 包含 [GNOME 43](https://www.gnome.org/) 的 beta 版本（我们预计 GNOME 43 的最终版本将在几周内发布。）。GNOME 43 在设置中新增了一个新的设备安全面板，为用户提供有关此系统的硬件和固件安全性的信息。在先前版本的基础上，更多 GNOME 核心应用程序已移植到最新版本的 GTK 工具包，提供了改进的性能和现代外观。

### 其他更新

Fedora Linux 现在正式支持 Raspberry Pi 4，包括图形加速。在其他 ARM 新闻中，Fedora Linux 37 Beta 放弃了对 ARMv7 架构（也称为 arm32 或 armhfp）的支持。

我们正准备将我们最受欢迎的两种变体：[Fedora CoreOS](https://getfedora.org/coreos/) 和 [Fedora Cloud Base](https://cloud.fedoraproject.org/) 推广到正式版本（Fedora Edition）。Fedora 正式版本是我们针对特定用例的旗舰产品。

!!! note "备注"  
    Fedora Edition 是指可在 [getfedora.org](https://getfedora.org/) 直接下载获得的 Fedora Linux 官方正式版本，目前包括 [Fedora Workstation](https://getfedora.org/en/workstation/)、[Fedora Server](https://getfedora.org/en/server/) 和 [Fedora IoT](https://getfedora.org/en/iot/)。

为了跟上密码学的进步，Fedora 37 Beta 版本引入了 *TEST-FEDORA39* 策略，该策略预览了为 Fedora Linux 39 计划的更改。新策略包括弃用 SHA-1 签名。

当然，还有编程语言和库的常规更新：Python 3.11、Perl 5.36、Golang 1.19 等等！

## 我们需要测试

由于这是一个 Beta 版本，我们预计你可能会遇到错误或功能缺失。要报告测试期间遇到的问题，请通过[测试邮件列表](https://lists.fedoraproject.org/archives/list/test%40lists.fedoraproject.org/)或 Matrix 上的 [`#quality`](https://matrix.to/#/#social:fedoraproject.org?web-instance[element.io]=chat.fedoraproject.org) 频道（已桥接到 [Libera Chat](https://libera.chat/) 上的 `#fedora-qa`）联系 Fedora QA 团队。随着测试的进行，我们会在 [AskFedora](https://ask.fedoraproject.org/) 上跟踪常见问题。

有关有效报告错误的提示，请阅读[如何提交错误](https://docs.fedoraproject.org/en-US/quick-docs/howto-file-a-bug/)。

## 什么是 Beta 版本？

Beta 版本是代码完整的，并且与最终版本非常相似。如果你花时间下载并试用 Beta，你可以检查并确保对你很重要的事务在新版系统上正常工作。你发现和报告的每一个错误不仅可以帮助你，还可以改善全球数百万 Fedora Linux 用户的体验！我们可以一起让 Fedora 坚如磐石。我们有一种协调新功能并尽可能向上游推送修复的文化。你的反馈不仅改进了 Fedora Linux，还会改进 Linux 生态系统和整个自由软件。

## 更多信息

有关 Fedora Linux 37 Beta 版新功能的更多详细信息，你可以查阅 [Fedora Linux 37 Change set](https://fedoraproject.org/wiki/Releases/37/ChangeSet)。它包含了有关此版本附带的新软件包和改进的更多技术信息。