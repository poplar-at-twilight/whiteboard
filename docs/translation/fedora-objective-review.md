---
comments: true
tags:
  - 新闻
  - Fedora
  - 社区动态
---

# 任务目标综述：不可变变体在使用中的 Fedora Linux 占大多数

## 译文信息

- 源文：[Objective Review: Immutable variants are the majority of Fedora Linux in use](https://discussion.fedoraproject.org/t/objective-review-immutable-variants-are-the-majority-of-fedora-linux-in-use/79288/1)
- 类型：论坛讨论贴
- 作者：[Matthew Miller](https://discussion.fedoraproject.org/u/mattdm)
- 许可证：未知
- 译者：暮光的白杨
- 日期：2023-03-27

----

## 正文

*我们正在制定 [Fedora Strategy 2028]——我们的下一个五年计划。我们现在正在核查这些**目标**及其相关**影响**。请阅读[此指南]，了解有关当前规划阶段的详细信息。*

该目标是“Fedora 引领 Linux 发行版开发”**主题**和*技术创新与领导力***重点领域**的一部分。有关此重点领域的一般讨论，请参阅 [Fedora Strategy 2028：重点领域综述（技术创新与领导力）]。

[Fedora Strategy 2028]: https://discussion.fedoraproject.org/t/fedora-strategy-2028-a-topic-index-for-our-planning-process/46733
[此指南]: https://discussion.fedoraproject.org/t/fedora-strategy-2028-focus-area-review-guidelines/46888
[Fedora Strategy 2028：重点领域综述（技术创新与领导力）]: https://discussion.fedoraproject.org/t/fedora-strategy-2028-focus-area-review-technology-innovation-leadership/79286

### 目标和影响

**目标**：**不可变变体**[^immutable]在使用中的 Fedora Linux 占大多数。  
**影响**：新的工作方式带来刺激和新活力。

[^immutable]: 不可变变体（Immutable variants）指的是 Fedora Linux 基于 [rpm-ostree](https://coreos.github.io/rpm-ostree/) 理念的各个分支版本（如 Silverblue、CoreOS 等）。

“不可变”或“基于镜像[^image]”的 Linux 系统有很强的趋势。事实上，Linux Weekly News 甚至预测 2023 年将是“**不可变发行版之年**”：

[^image]: en: image based

> 经典的 Linux 发行版紧随之前 Unix 系统的脚步；具有适当权限的用户可以随时更改系统的任何方面。不过近几年来，我们已经看到了从这种模式向（至少部分是）不可改变的系统的转变。Android 可以说是不可变系统最突出的例子。Android 的系统核心只能通过更新和重启的方式进行更改——并且以前的版本仍然存在以备不时之需。  
> 诸如 Fedora [Silverblue] 的发行版也一直在探索不可变领域。即将推出的 SUSE [自适应 Linux 平台（ALP）][^link-broken]和刚刚发布的基于 Ubuntu 的 [Vanilla OS] 系统一样，都是基于一个不可变的内核。其他人似乎很可能会效仿，也许会使用 [Image-Based Linux 2022 年峰会]制定的蓝图。到今年年底，可能会有许多不可变的替代方案可供使用——并用于实际工作。  
> 来自：<https://lwn.net/Articles/918790/>

[^link-broken]: LWN 文章中的链接已损坏，另见 <https://documentation.suse.com/alp/micro/html/alp-micro/concept-alp.html> 和 [The SUSE ALP Bedrock Guide | General description](https://documentation.suse.com/alp/bedrock/html/alp-bedrock/concept-alp.html)。

[Silverblue]: https://silverblue.fedoraproject.org/about
[自适应 Linux 平台（ALP）]: https://documentation.suse.com/alp/all/single-html/alp/index.html
[Vanilla OS]: https://vanillaos.org/
[Image-Based Linux 2022 年峰会]: https://lwn.net/Articles/912774/

Fedora 长期以来一直是这方面的领导者（[Fedora Atomic Host] 是 Fedora 22 的一项功能！），而 Fedora [CoreOS] 目前是我们第四大最受欢迎的变体（仅次于 [Workstation]、[Cloud] 和 [Server]）。使用 Silverblue（以及 [Kinoite] 和朋友们）的人数较少，但在媒体和发烧友方面的影响确实很大。

但与此同时：我们的传统系统*运行良好*。这使我们处于“[局部最大值]（The Local Maximum）”的处境——很难得到新的东西[^innovator]。通过这一目标，Fedora 委员会希望明确承诺投资于不可变方法，并着眼于使其成为大多数用例的默认方法。

[^innovator]: 另请参阅：[创新者的困境](https://archive.org/details/innovatorsdilemm0000chri/mode/2up)。

[Fedora Atomic Host]: https://fedoraproject.org/wiki/Changes/AtomicHost
[CoreOS]: https://getfedora.org/coreos/
[Workstation]: https://getfedora.org/en/workstation/
[Cloud]: https://cloud.fedoraproject.org
[Server]: https://getfedora.org/en/server/
[Kinoite]: https://kinoite.fedoraproject.org/
[局部最大值]: https://52weeksofux.com/post/694598769/the-local-maximum

### 我们现在的目标

对于此**目标**和相关**影响**，验证：

1. 如果实现了该影响，则可以合理地预期活跃的 Fedora 贡献者会增加。
2. 该目标的成功在逻辑上会产生预期的影响。
3. 该关联相当*充分*——也就是说，它代表了产生影响所需的一切。
4. 虽然可能有其他方法可以产生类似的影响，但所选的目标是目前适合 Fedora 的目标。
5. 措辞准确、清晰。目标是具体的，影响是（至少有一点）鼓舞人心的。它们一起适合这个重点领域。

如果你可以改进[这篇文章]顶部较长的解释性段落，那也很有帮助！

*如[路线图]所述，这篇文章将在一个月后关闭。*

[这篇文章]: https://discussion.fedoraproject.org/t/objective-review-immutable-variants-are-the-majority-of-fedora-linux-in-use/79288/1
[路线图]: https://discussion.fedoraproject.org/t/fedora-strategy-2028-a-topic-index-for-our-planning-process/46733#objective-impact-feedback-and-discussion-6