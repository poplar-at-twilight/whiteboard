---
comments: true
tags:
  - 新闻
  - KDE
  - Wayland
---

# 让我们来谈谈 Wayland 这件事吧

- 原文：[So let’s talk about this Wayland thing](https://pointieststick.com/2023/09/17/so-lets-talk-about-this-wayland-thing/)
    - 作者：[Nate](https://pointieststick.com/author/pointieststick/)
    - 许可证：[CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)（[协议副本](./assets/kde-wayland-license.png)）
    - 译者：暮光的白杨
    - 日期：2023-09-18

----

> [Nate Graham]（Nate）是 KDE 项目的开发者之一。

[Nate Graham]: https://discuss.kde.org/u/ngraham/summary

[Wayland] 经常出现在 “Plasma Wayland 会话中修复了某错误”，“Plasma Wayland 会话已支持某功能”等消息之中。最近，[Fedora KDE 宣布将在 Fedora 40 中废弃 Plasma X11 会话，只提供 Plasma Wayland 会话]，这让 Wayland 成为新闻焦点。最近，我读到了很多关于此事的紧张和担忧。

[Wayland]: https://wayland.freedesktop.org/
[Fedora KDE 宣布将在 Fedora 40 中废弃 Plasma X11 会话，只提供 Plasma Wayland 会话]: https://fedoraproject.org/wiki/Changes/KDE_Plasma_6#Feedback

那么今天我们就来聊一聊吧！

## 什么是 Wayland？

Wayland 是[一组协议]，用于管理混成器[^farseerfc]（compositor）如何在屏幕上绘制内容，以及应用程序如何与混成器在屏幕上绘制的基础内容设施交互。它类似于控制网络浏览器和电子邮件客户端发送和接收网页和数据的 HTTP 和 SMTP 协议。

[^farseerfc]: 扩展阅读：[桌面系统的混成器简史]

[一组协议]: https://wayland.app/protocols/
[桌面系统的混成器简史]: https://farseerfc.me/zhs/brief-history-of-compositors-in-desktop-os.html

Wayland 还在一组名为 `libwayland-client` 和 `libwayland-server` 的极轻量级库中包含这些协议的实现，并提供稳定且版本化的 API。应用程序和混成器（例如 KDE 的 [KWin] 和 GNOME 的 [Mutter]）使用这些 API 来完成任务。

[KWin]: https://userbase.kde.org/KWin
[Mutter]: https://gitlab.gnome.org/GNOME/mutter

## Wayland 为何存在？

简而言之，因为 [X11]——wayland 正在取代的东西——已经死了。

[X11]: https://www.x.org/wiki/Releases/7.7/

X11 多年来一直处于维护模式，最近除了对 [XWayland] 兼容性系统（允许 X11 应用程序使用 Wayland 混成器）的变更外，根本[没有任何真正的发展]。像窗口服务器这样的核心系统得不到维护是个大问题，因为这意味着没有错误修复、没有安全补丁，也没有新功能让它跟上不断变化的世界。

[没有任何真正的发展]: https://gitlab.freedesktop.org/xorg/xserver/-/commits/master
[XWayland]: https://wayland.freedesktop.org/xserver.html

## X 为什么死了？

X11 的基本开发模式是拥有一个重量级的窗口服务器，即 Xorg，它可以处理一切事务，每个人都可以使用它。理论上可以有其他的 [X] 服务器，而且在不同的时间点上也[有过]，但实际上编写一个不是旧服务器的分叉（fork）的新服务器几乎是不可能的。每个人都强烈倾向于使用单一的 X 服务器作为标准，当有更好的分叉出现时，大家就会一致地从一个服务器迁移到另一个服务器，因为这样做很方便。说它方便，是因为它集中了有限的开发资源，而且当 X 服务器上添加了某个功能时，所有人都能自动获得该功能。

但这样做有一个缺点：因为每个人都在使用 Xorg，所以为支持任何用例而添加的任何功能都可能会破坏其他人使用的其他所有功能，而且这种情况经常发生。错误修复经常会导致其他人正在使用的非常见功能发生[回退]。

[X]: https://www.x.org
[回退]: https://en.wikipedia.org/wiki/Software_regression
[有过]: https://en.wikipedia.org/wiki/XFree86

从本质上讲，Xorg 变得太大、太复杂和太脆弱，如果不冒着破坏整个 Linux 生态系统的风险，就无法触及问题所在。如今它之所以稳定，是因为多年来它基本上处于冻结状态。但这种稳定与停滞并存。众所周知，在科技界，无法适应的项目就会消亡。依赖它们的项目也会随之消亡。

## Wayland 会好点吗？

Wayland 是由 [X] 开发人员提出的，他们希望避免重复自己的错误。除了许多技术差异之外，wayland 通过一套最小化的协议和两个极其精简的客户端和服务器库，将所有繁重的工作都委托给了各个混成器，它们成为了各自[环境]中的窗口服务器。一个混成器中添加的新功能不会破坏其他混成器的稳定。混成器还可以自由地通过私有协议实现仅针对该混成器的应用程序可以使用的标准协议之外的新功能。

[环境]: https://wiki.archlinux.org/title/Desktop_environment

## 等等，听起来好像很糟糕

Wayland 确实存在不少问题。在我看来，由于它是由疲惫不堪的 X 开发人员发明的，因此在另一个方向上走得太远了。Wayland 的最小核心协议缺乏大部分非简单应用和桌面实际运行所需的功能，例如屏幕锁定、屏幕共享、跨应用窗口激活、非整数缩放等等。所有的混成器都需要自己想办法实现这些功能。这不仅浪费了开发时间，也不利于那些没有专业图形开发人员的小型团队。这些都是真实存在的问题，我们不应该将其掩盖起来。

## 是的，但是有解决方案

随着时间的推移，最小的核心协议已经扩展到涵盖 Linux 桌面和复杂应用程序运行所需的内容。其中大部分工作都是最近在 KDE 的推动下，由 [Blue Systems] 和 [Valve Software] 资助完成的。因此，你在一两年前读到的大多数关于 Wayland 缺少这个或那个（例如分数缩放、屏幕共享或全局快捷方式）东西的抱怨，在今天看来很可能是错误的。

[Blue Systems]: https://blue-systems.com/
[Valve Software]: https://www.valvesoftware.com

此外，[wlroots] 也正在解决工作分散的问题，它是一个 Wayland 实现库，你可以用它来构建一个 Wayland 混成器。我们没有在 KDE 的 KWin 混成器中使用它，因为在 wlroots 出现之前，我们已经自己完成了大部分工作，但它对现在从头开始编写新混成器的任何人来说是一个很大的好处。我们将来也有可能将 KWin 移植到 wlroots 中。

[wlroots]: https://gitlab.freedesktop.org/wlroots/wlroots

## 为什么采用新方案要花这么长时间？

事实上，Wayland 的最小核心协议使其无法完全取代它想要取代的东西，这是其作者在架构设计上的一个错误决定，这削弱了它在 2008 年发布时被快速采用的可能性。在 [Systemd] 和 [PipeWire] 等新项目中，我们并没有看到同样的问题，它们的采用速度要快得多。

[Systemd]: https://systemd.io/
[PipeWire]: https://pipewire.org/

不幸的是，通过批准程序引导新协议来解决这个问题是一项累人的政治活动。这需要与其他项目的人员达成共识和妥协，因为他们从根本不同的角度来看待你试图解决的问题。有人甚至可能不同意这个问题值得解决。[琐碎的讨论]会使得交流脱轨，让每个人情绪低落并停止工作。在很长一段时间里，因为 X 还没有 [EOL]，所以推进的紧迫感很低。

[琐碎的讨论]: https://en.wikipedia.org/wiki/Law_of_triviality
[EOL]: https://en.wikipedia.org/wiki/End-of-life_product

就这样拖了 15 年多，导致 Wayland 的丑事一直被公之于众。真是糟透了。但是……我们现在做到了。标准协议现在几乎满足了所有人的所有需求。我们正在优先积极解决剩下的几个明显疏漏（如屏幕颜色校准）。

## “我们做到了吗？”

Plasma 和 KDE 应用程序在 Wayland 上运行良好，**尤其是**在即将发布的 Plasma 6 版本中。就像我说的，虽然仍有一些疏漏，但这些漏洞最近正在被迅速填补。

大多数非 Wayland 原生的第三方应用程序也能通过 XWayland 兼容层正常运行。但也有一些应用程序无法正常运行，因为它们以需要 X11 支持的方式深度集成到系统的某些部分。这些程序需要进行移植，以使用新的 Wayland API。

当 Wayland 还只是个玩具时，很多应用开发者已经习惯了不理睬它的新闻，而不去做移植工作。现在，Wayland 不再是玩具了，很多人都因为他们突然急需将自己的应用程序移植到 Wayland 上而感到措手不及。这是可以理解的。但这次是真的，**现在**就是移植的时候了。对于任何仍不够完善、需要修订的协议，都需要应用程序开发人员提供意见，对其进行修订，甚至提出新的协议。这个过程需要很长时间，所以越早开始越好。但它不会消失。

## 这让我们想到了 Fedora KDE

Fedora 一直是一个“前沿”发行版，一旦新技术**准备就绪**，它就会采用新技术来推动 Linux 的技术发展，从而使其以一种前所未有的方式迅速改进。Fedora 是第一个采用 Systemd、[PulseAudio] 和 PipeWire 的发行版。它是第一个默认使用 Plasma Wayland 会话的发行版。现在，Fedora KDE 希望成为第一个完全放弃 Plasma X11 会话并让每个人都使用 Plasma Wayland 会话的发行版。

[PulseAudio]: https://www.freedesktop.org/wiki/Software/PulseAudio/

很显然，Fedora 的目标受众是那些对变革充满激情的人。如果你是这样的人，那么所有这些项目都应该是令人兴奋和很酷的！如果不是……那么 Fedora 不适合你。那也没关系。Linux 世界大约有五百个发行版。你说什么？其中只有 20 个是好的？好吧，很有道理，但即使这个荒诞的数字是准确的，仍有 19 个发行版不是 Fedora！重新安装操作系统固然令人不快，但选择适合自己需求和喜好的操作系统才是最重要的。有了选择，就有责任做出明智的选择。

也许你担心 Fedora 是预示着一切的发展方向的“煤矿中的金丝雀[^canary]”。你的想法没有错，但过渡仍需要时间。像 Ubuntu LTS 或 Debian Stable 这样有意推出旧版软件的发行版，会让你继续使用 X11 多年。Arch Linux 可能也会继续使用 X11 一段时间。你还有其他选择。等到 Ubuntu LTS 也移除 X11 时，一切都将准备就绪。

[^canary]: 暗指笼中的金丝雀，矿工们将它们带入矿井隧道。如果矿井中聚集一氧化碳等危险气体，这些气体会在杀死矿工之前先杀死金丝雀，从而发出立即离开隧道的警告。  
    （惯用语）对不利条件的敏感性使其成为此类条件的有用早期指标的东西；警告由于健康或福利恶化而带来更大危险或麻烦的事物。

## 结语

Wayland 是 X11 的替代品，而 X11 已经死了。尽管开发过程很艰难，但它已经为 Plasma 和 KDE 应用程序做好了足够的准备，因此 Fedora KDE 正在大力推动它。许多第三方应用程序已经原生支持 Wayland，但也有很多不是，它们需要花些功夫才能移植到 Wayland 上。如果 Wayland 还缺少开发者需要的任何东西，他们需要站出来参与添加它的过程。这个过程正在发生，并且不会停止发生。我们需要共同努力，使其更快、更顺利地发生。

----

## 评论区

<center>

[点此前往评论区](https://pointieststick.com/2023/09/17/so-lets-talk-about-this-wayland-thing/){ .md-button }

</center>