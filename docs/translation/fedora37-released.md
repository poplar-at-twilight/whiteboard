---
comments: true
tags:
  - 新闻
  - Fedora
  - GNOME
---

# Fedora Linux 37 现已正式发布

## 文章信息

- 原文：[Announcing Fedora Linux 37](https://fedoramagazine.org/announcing-fedora-37/)
- 作者：[Matthew Miller](https://fedoramagazine.org/author/mattdm/)
- 许可证：[CC-BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/)
- 译者：暮光的白杨
- 日期：2022-11-15

-----

![01](./images/2022-11/f37-release-1-1024x433.jpg)

今天，我很高兴与大家分享数千名 Fedora 项目贡献者辛勤工作的成果：Fedora Linux 37 发布了！让我们看看最新版本为你带来了什么。与往常一样，在从以前的版本升级之前，你应该确保你的系统已经更新至最新状态。等不及要开始了吗？请一边阅读一边[下载](https://getfedora.org/)！

## 新版本

**Fedora Edition** 是针对特定“市场”的旗舰产品。在 Fedora Linux 37 中，我们添加了两个新版本。Fedora CoreOS 是你可能记得的 Atomic Host 的继承者。从 Project Atomic 和原始 CoreOS 工作中汲取灵感，Fedora CoreOS 提供了一种自动更新机制，适用于托管基于容器的工作负载。通过原子更新和轻松回滚，它让你的基础架构更加令人安心。

Fedora Cloud 也作为一个 Fedora Edition 回归。Fedora 云工作组重新活跃起来。Cloud 提供了一个很好的 Fedora 基本系统，可以在你最喜欢的公共或私有云中运行。 AMI 将于本周晚些时候在 AWS Marketplace 中提供，社区渠道现已可用。请在此[网站](https://getfedora.org/en/cloud/)上查看其他云提供商或你自己的云服务提供的镜像文件！

## 桌面改进

Fedora Workstation 专注于桌面体验。与往常一样，Fedora Workstation 搭载了 GNOME 的最新版本。GNOME 43 在设置中新增了一个新的设备安全面板，为用户提供有关系统硬件和固件安全性的信息。在上一个版本的基础上，更多 GNOME 核心应用程序已移植到最新版本的 [GTK](https://www.gtk.org/) 工具包，提供改进的性能和现代外观。

在此版本中，我们进行了一些更改，以允许你稍微精简安装。 我们将 Firefox 浏览器的语言包拆分为子包。 这意味着如果你不需要本地化，你可以删除 `firefox-langpacks`。用于帮助其他软件包生成多语言文本的工具，gettext 的运行时软件包，被分成一个单独的、可选的子包。

当然，我们构建的不仅仅是 Fedora Edition。此外还有面向各种受众和用例的 [Fedora Spins](https://spins.fedoraproject.org/) 和 [Labs](https://labs.fedoraproject.org/)，例如为计算神经科学提供工具的 [Fedora Comp Neuro](https://labs.fedoraproject.org/en/comp-neuro/)，以及提供轻量级桌面环境的 [Fedora LXQt](https://spins.fedoraproject.org/en/lxqt/) 等桌面环境。此外，请不要忘记我们还支持其他架构，如 [ARM AArch64、Power 和 S390x](https://alt.fedoraproject.org/alt/)。

## 系统管理增强

Fedora Server 现在提供 KVM 磁盘镜像，使在虚拟机中运行 Server 更容易。如果你禁用了 SELinux（没关系——我们仍然爱着你！），你可以在影响较小的情况下重新开启它。 `autorelabel` 现在以并行方式运行，使 `fixfiles` 操作更快。

为了跟上密码学的进步，Fedora 37 引入了 TEST-FEDORA39 策略，用于预览为未来版本计划的更改。新策略包括放弃 SHA-1 签名。研究人员早就知道这种散列（就像之前的 MD5）用于许多安全目的是不安全的。

将来，我们可能会从 Fedora Linux 可接受的安全算法列表中删除 SHA-1（正如 TEST-FEDORA39 这个名称所暗示的那样，可能最快在明年）。但是，我们知道今天仍在使用 SHA-1 哈希。新策略可帮助你立即测试你的关键应用程序，以便你做好准备。请尝试一下，让我们知道你在哪里遇到问题。

说到密码学，openssl1.1 包现已弃用。它将保持可用状态，但我们建议你更新代码以使用 openssl 3。

## 其他更新

Fedora Linux 现已正式支持 Raspberry Pi 4，包括图形加速。在其他 ARM 新闻中，Fedora Linux 37 放弃了对 ARMv7 架构（也称为 arm32 或 armhfp）的支持。

基于我们的“[第一](https://docs.fedoraproject.org/en-US/project/#_first)”原则，我们更新了关键的编程语言和系统库包，包括 Python 3.11、Golang 1.19、glibc 2.36 和 LLVM 15。

我们很高兴你能试用新版本！请立即转到 [getfedora.org](https://getfedora.org/) 并下载 ISO 文件。或者，如果你已经在运行 Fedora Linux，请按照[简易升级说明](https://docs.fedoraproject.org/en-US/quick-docs/upgrading/)进行升级操作。有关 Fedora Linux 36 中新功能的更多信息，请参阅[发行说明](https://docs.fedoraproject.org/en-US/fedora/f36/release-notes/)。

## 万一出现问题……

如果你遇到了问题，请访问我们的 [Ask Fedora](https://ask.fedoraproject.org/) 用户支持论坛。这包括[常见问题](https://ask.fedoraproject.org/c/common-issues/141/none)的类别。

## 感谢所有人

感谢在此发布周期中为 Fedora 项目做出贡献的数千人。我们为 Fedora 社区拥有您的存在感到欣喜。