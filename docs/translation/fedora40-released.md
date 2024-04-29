---
comments: true
tags:
  - 新闻
  - Fedora
  - GNOME
---

# 噢！我们四十了！（Fedora Linux 40 现已发布）

- 译文信息：
    - 原文：[OMG! We’re at forty! (Announcing the release of Fedora Linux 40)](https://fedoramagazine.org/announcing-fedora-linux-40/)
    - 作者：[Matthew Miller](https://fedoramagazine.org/author/mattdm/)
    - 许可证：[CC-BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/)
    - 日期：2024-04-23
    - 译者：暮光的白杨

----

> ![](./images/2024-04/fedora/Fedora_Linux_40_release-1024x433.jpg)
> 图片由 Consolation Obazee 提供

哇哦，感觉这个数字很大啊！我很自豪地宣布 Fedora Linux 第 40 个版本发布了，这是一个属于我们所有人的社区构建和社区维护的操作系统。我也很高兴地注意到，我们又回到了按时发布的轨道上。感谢所有的 Fedora 贡献者，是他们让这一切成为可能，并且再次使它成为我们有史以来最好的一个。

这对我个人来说也是一个激动人心的数字，因为这标志着我作为 Fedora 项目负责人发布的第 20 个版本。在过去的十年中，我们经历了很多，我非常高兴地看到我们的社区蓬勃发展和壮大。除了许多熟悉的名字和面孔，看到新一代的活力和想法也让人兴奋。在某些情况下，这简直就是新一代，因为你们中的许多人都是伴随着 Fedora 成长起来的。但无论年龄大小，我都为我们建立了这样一个热情友好的社区而感到自豪，我们将继续努力提高我们的包容性、多样性和可及性。

但无论如何。是时候看看我们在 Fedora Linux 40 为你带来了什么了！如果你已经有了自己的系统，[升级 Fedora 到新版本]也很简单。如果你是新用户，或者只是好奇，请前往 [Get Fedora] 了解安装选项。

[升级 Fedora 到新版本]: https://docs.fedoraproject.org/en-US/quick-docs/upgrading-fedora-new-release/
[Get Fedora]: https://fedoraproject.org/

## 桌面新闻

Fedora Workstation 采用 GNOME 桌面环境，现已更新至 46.0 版本。请阅读《[Fedora Workstation 40 中有哪些新增功能？][exlink1]》了解新版本的亮点。

[exlink1]: ./fedora40-whatnew.md

Fedora KDE Spin 现在搭载了 KDE Plasma 6，并且可以与 Wayland 一起运行，开箱即用。有关更多信息，请阅读《[Fedora KDE 40 中的新增功能][exlink2]》。

[exlink2]: ./fedora40-whatnew-kde.md

我们还将正式恢复 "Fedora Atomic Desktop" 品牌，用于我们所有使用 ostree 或基于镜像配置的变体。我们的技术并非真正 "不可变（immutable）"，因此这提供了一个更好的分组。有关于此，请阅读《[Fedora Atomic Desktops 简介][exlink3]》。简而言之，我们将继续保留 Fedora Silverblue 和 Fedora Kinoite。而其他桌面环境变体将重命名为 Fedora Sway Atomic 和 Fedora Budgie Atomic 等格式。

[exlink3]: ./fedora-atomic-de-intro.md

## AI 开发工具

Fedora Linux 40 预装了我们的首个 [PyTorch] 软件包。PyTorch 是一个流行的深度学习框架，没有它的帮助，很难可靠地安装正确版本的驱动程序和库。目前的软件包只支持在 CPU 上运行，不支持 GPU 或 NPU 加速，但这只是第一步。我们的目标是提供一个完整的堆栈，包含 PyTorch 和其他流行工具，可以在各种硬件上即开即用。

[pytorch]: https://pytorch.org/

我们还将推出 ROCm 6，这是一款为 AMD 显卡提供加速支持的开源软件。我们计划在未来的版本中为 PyTorch 启用这一功能。

## 其他更新

像往常一样，我们使用更新的编译器和库重建了发行版中的所有内容（当然，开发人员也可以使用这些更新的工具）。这些更新带来了错误修复、安全改进和性能提升。

当然，还有数以百计的 Fedora 打包人员和测试人员，他们努力从数以千计的上游项目中整合最新版本的开源软件。而这些项目又是由无数的开发者和贡献者共同完成的，他们的工作涉及营销、设计、文档、代码、质检、翻译、交流、活动、管理、基础设施、安全等等。再次感谢让 Fedora 大放光彩的每一个人，以及所有为构建整个自由开源软件世界而付出努力的人。

## 说到更新……

今天还有几个重要的发布日错误修复和安全更新。如果你从较早的 Fedora Linux 版本升级，你将获得这些更新。对于新安装的系统，请确保尽快检查并应用更新。

## 万一出现问题……

如果你遇到问题，请访问我们的 [Ask Fedora] 用户支持论坛。其中有一个涵盖[常见问题]的类别。

[Ask Fedora]: https://ask.fedoraproject.org/
[常见问题]: https://discussion.fedoraproject.org/tags/c/ask/common-issues/82/none/f40

## 或者如果你只是想说声 “Hello”……

欢迎访问 [Fedora Discussion 上的“虚拟饮水机（virtual watercooler）”][watercooler]并加入对话，分享有趣的内容并介绍自己。

[watercooler]: https://discussion.fedoraproject.org/c/fun/8

另外，请记住我们的年度贡献者大会 [Flock To Fedora] 即将召开！Flock 今年八月将在纽约罗切斯特举行。[会议提案征集仍在进行中]，如果你有想要分享或参与的内容，或者有兴趣成为一名贡献者，或者认为你可能是，我们将很高兴在那里见到你！

[Flock To Fedora]: https://flocktofedora.org/
[会议提案征集仍在进行中]: ./fedora-flock-2024-cfp.md