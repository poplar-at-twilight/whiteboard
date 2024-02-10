---
comments: true
tags:
  - 新闻
  - GIMP
---

# GIMP 2022 年度报告

## 译文信息

- 原文：[2022 annual report](https://www.gimp.org/news/2023/01/29/2022-annual-report/)
- 作者：[Jehan](https://www.gimp.org/author/jehan.html)
- 许可证：[CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
- 译者：暮光的白杨
- 日期：2023-01-30

----

追寻[一年前](https://www.gimp.org/news/2021/12/31/gimp-2021-annual-report/)开始的新传统，这是我对过去 2022 年的报告。

> ![cover](./images/2023-01/GIMP-2023-january-report.jpg)
> *《Go 2023》，由 [Aryeom](https://film.zemarmot.net/) 创作，基于 CC BY-SA 4.0 进行授权使用。*

## 统计数据

在 2022 年，我们：

- 发布了 **1 个稳定版本**（[GIMP 2.10.32](https://www.gimp.org/news/2022/06/14/gimp-2-10-32-released/)）
- 发布了 **3 个开发版本**（[GIMP 2.99.10](https://www.gimp.org/news/2022/02/25/gimp-2-99-10-released/)、[2.99.12](https://www.gimp.org/news/2022/08/27/gimp-2-99-12-released/) 和 [2.99.14](https://www.gimp.org/news/2022/11/18/gimp-2-99-14-released/)）。
- 在不稳定的开发分支（2.99.x，未来的 3.0）上进行了 **1461 次 commit**，在稳定的开发分支（2.10.x）上进行了 **276 次 commit**。
- 主存储库有 **87 个贡献者**，包括（有些人同时属于数个类别）：  
    - 35 位开发人员  
    - 47 位翻译人员  
    - 26 位改进构建或资源贡献者（图标、主题、代码内文档）
- **7 名核心开发人员**在 GIMP 的主存储库中贡献了 10 次或更多次 commit：  
    - Jehan: 649 commits  
    - Jacob Boerema: 64 commits  
    - Nikc: 50 commits  
    - Daniel Novomeský: 25 commits  
    - lloyd konneker: 25 commits  
    - Lukas Oberhuber: 18 commits  
    - Niels De Graef: 15 commits
- 10 位贡献者向 [babl](https://gegl.org/babl/) 提交了 **115 次 commit**，其中 3 位开发人员贡献了 10 次或更多次 commit：  
    - Øyvind Kolås: 86 commits  
    - Axel Viala: 10 commits  
    - Jehan: 10 commits
- 32 位贡献者向 [GEGL](https://gegl.org/) 提交了 **138 次 commit**，其中 5 位开发人员贡献了 5 次或更多次 commit：  
    - Øyvind Kolås: 47 commits  
    - Behnam Momeni: 9 commits  
    - Michael Drake: 7 commits  
    - Thomas Manni: 7 commits  
    - Jehan: 5 commits
- 两位贡献者（主要是 Øyvind Kolås）向 [ctx](https://ctx.graphics/) 提交了 **1042 次 commit**。
- 29 名贡献者在 `gimp-help`（我们的手册）中提交了 **492 次 commit**，其中 11 人贡献了 10 次或更多次 commit（此列表包含了文档编写人员、构建维护人员和翻译人员）：  
    - Jacob Boerema: 229 commits  
    - Anders Jonsson: 47 commits  
    - Rodrigo Lledó: 38 commits  
    - Jehan: 28 commits  
    - Jordi Mas: 25 commits  
    - Tim Sabsch: 19 commits  
    - Nathan Follens: 17 commits  
    - Marco Ciampa: 16 commits  
    - Yuri Chornoivan: 15 commits  
    - Andre Klapper: 13 commits  
    - Hugo Carvalho: 11 commits  
- 3 位贡献者（主要是 Lukas Oberhuber）在 `gimp-macos-build`（我们的 macOS 构建）中提交了 **178 次 commit**。
- 6 位贡献者在我们的 Flathub/Flatpak 包的稳定分支中提交了 **33 次 commit**，在 beta 分支中提交了 **23 次 commit**；其中包括 4 位核心贡献者：Jehan、Ondřej Míchal、Hubert Figuière 和 Daniel Novomeský。
- 10 位贡献者（主要是 Jehan）向 GIMP 的网站（比如 `gimp.org`）提交了 **227 次 commit**。
- 4 位贡献者为我们新的[开发者网站](https://developer.gimp.org/)贡献了 **158 次 commit**：  
    - Jehan: 104 commits  
    - Pat David: 38 commits  
    - Robin Swift: 15 commits  
    - Lukas Oberhuber: 1 commit  
- **修复了 178 份报告，合并了 206 个合并请求**至我们于 2022 发布的版本中。我们还处理、分类、答复、努力修复**数百份**报告……
- GIMP 贡献者在我们使用的各种其他项目（至少包括 GLib、GTK、Cairo、meson、Mirrorbits……）中贡献了许多补丁，并向其他项目报告的无数问题。
- 还有更多！

与往年相比：

- 工作总量相当接近。虽然这种趋势在一年前就已经开始，但贡献者们显然更多地转向开发分支（未来的 3.0 版本），现在该分支获得的 commit 占到了 84% 的提交量（去年是 74%），而稳定分支则真正进入了只维护的模式。
- GEGL 上的工作变少了，但 babl 上的工作变多了。最近关于自动 LUT 创建和 SIMD 优化的工作说明了这一点。
- ctx 保持活跃开发状态。
- 虽然 Øyvind 和我自己[^1]仍然是 2 名主要贡献者，但我们周围有更多人明显地在释放他们的光芒。我很高兴看到更多的贡献者留下来。
- Jacob 在文档方面做了更多工作，其质量确实在提高。

## 2022 年的杰出发展

### babl 和 GEGL

在我们的图形引擎方面，babl 中用于颜色转换的[自动 LUT 创建](https://www.gimp.org/news/2022/02/25/gimp-2-99-10-released/#automatic-lut-creation-for-conversions-in-babl)显然是向前迈出的一大步，它在 GIMP 2.99.10（然后是稳定版本 2.10.32）中引入。

同时，babl、GEGL 和 ctx 都得到了不错的 [SIMD 优化](https://www.gimp.org/news/2022/02/25/gimp-2-99-10-released/#simd-builds-for-x86_64-and-arm-neon-ctx-babl-and-gegl)，从而实现了不错的性能提升。

和往常一样，Øyvind Kolås 确实做得非常出色。

同样有趣的是，“GEGL 插件”的概念是如何在 2022 年兴起的。它实际上只是指第三方 GEGL 操作，你只需将其安装在一个文件夹中，GIMP 将在下次重启时看到它们，包括所有 花哨的用户界面，例如带有拆分视图的画布预览（当我们拥有非破坏性图层效果时，这些操作也将可用！）。

在引领此类社区发展的人中，我们应该提到 [LinuxBeaver](https://github.com/LinuxBeaver?tab=repositories) 和 [Liam Quin](https://gitlab.com/barefootliam/gegl-pango-markup)。对于任何感兴趣的人，我建议阅读 Liam 编写的由 3 部分组成的教程（“[使用 GEGL 插件](https://barefootliam.blogspot.com/2022/10/gegl-plug-ins-for-gimp-part-one-using.html)”、“[GEGL 图形](https://barefootliam.blogspot.com/2022/12/gegl-plug-ins-for-gimp-part-two-gegl.html)”和“[编写 C 插件](http://barefootliam.blogspot.com/2023/01/gegl-plug-ins-for-gimp-part-three.html)”）。

### 检查 GIMP 路线图中的项目✅

我们自豪地勾选了 [GIMP 3.0.0 路线图](https://developer.gimp.org/core/roadmap/#gimp-30-development-branch-roadmap)中的几个项目。

在 2022 年的成就中，我们完成了……

- [x] 从 intltool 移植到仅 gettext（技术债务清理）；
- [x] 完成 meson 构建：autotools 构建仍然存在，但现在被认为是次要的；
- [x] 完成了多层选择的最后一项任务（自 [2020 年初开始](https://www.gimp.org/news/2020/11/06/gimp-2-99-2-released/#multi-layer-selection)的一项举措）。包括完全重写以前糟糕的[对齐和分布工具](https://www.gimp.org/news/2022/11/18/gimp-2-99-14-released/#align-and-distribute-tool-fully-reworked-interaction)中的交互。

这些是我们路线图中的 3 个巨大部分，我们很高兴将其标记为已完成（除了可能的 bug）。

在接近完成的工作：

- 我们几乎完成了“减少浮动选择”的动作（我们需要更多地考虑一些仍然存在的用例）。
- Wayland 支持有时仍然有点不稳定（即使忽略我们无能为力的所有问题——例如 Wayland 尚未实施的颜色管理——我们有奇怪的窗口问题），但它在 2022 年有所改善。
- API 工作确实在向前发展；Lloyd Konneker 在这方面提供了很多帮助。
- GTK+3 移植即将完成，因为我们最近正在处理最后烦人的警告（尽管它更像是 2023 年 1 月的事情！）。
- Space invasion：自从 CMYK 推送让我们更详细地查看特定代码片段以来，它的大部分内容已经完成。尽管还有很多工作要做，而且色彩科学有时是工作中非常令人头疼的部分。

现在任何关注我们[开发新闻](https://www.gimp.org/news/)的人都知道，还发生了很多事情。本报告不打算重复我们在各种新闻中已经写过的内容。

今年值得鼓励的一个特别贡献者是 Nikc，他最初带着几个补丁来到我们这里，然后提出了一个 [GSoC 项目](https://www.gimp.org/news/2022/06/03/cmyk-in-gsoc-2022/)，并决定留下来。多亏了他们，GIMP 中的 CMYK 支持发生了很多变化，我们的 “Space Invasion” 项目也取得了进一步的进展。他们现在是非常多产的核心贡献者。这意味着美好的未来！

### 打包

显然，我们的 **macOS** 支持从未如此出色：良好的持续集成、自动创建 DMG 包，现在我们甚至发布了[适用于 Apple Silicon 的软件包](https://www.gimp.org/news/2022/12/02/gimp-2.10.32-apple-silicon/)！此软件包的维护和更新质量非常出色。为此，我们特别感谢 Lukas Oberhuber。然而，我们的 macOS 软件包的总线因子仍然非常低，因此我们始终欢迎更多的贡献者。

在 **Windows** 端，在与 Microsoft 的开发人员关系团队联系后，GIMP 现在正式在 [Windows 商店](https://www.gimp.org/news/2022/06/18/gimp-2-10-32-on-microsoft-store/)中发布。这很棒，因为过去在那里分发了太多不受信任的软件包，现在它们似乎大部分都消失了，官方软件包以其非常好的评级使它们黯然失色。

在 **Flathub** (GNU/Linux) 上，由于 Ondřej Míchal 的帮助，我们现在在依赖版本检查中实现了自动化，因此负担越来越轻。flatpak 包团队也越来越大，有 4 个经常性贡献者。

### 基础设施

我们还对基础架构进行了一些更改，例如我们的镜像系统现在基于 *Mirrorbits*。这是我计划很快再谈的事情，所以我不会详细介绍。

在社区方面，我们的邮件列表已经停止，连同我们所在的所有 [GNOME 邮件列表](https://mail.gnome.org/archives/desktop-devel-list/2022-August/msg00004.html)都已停止。我们现在为社区[推荐](https://www.gimp.org/discuss.html#forums) 2 个论坛：

- 由自由与开源摄影社区 [pixls.us 主办的论坛](https://discuss.pixls.us/gimp/)。
- 由 GNOME 基金会的自由软件桌面项目 [GNOME project 主办的论坛](https://discourse.gnome.org/tag/gimp)。

### 网站

感谢 Jacob Boerema 的帮助，我们的[文档网站](https://www.gimp.org/news/2022/08/27/gimp-2-99-12-released/#documentation-website)得到了很多人的喜爱，具有了自动更新，统计显示……当然，内容正在接受严格审查以提高文档质量。与 2021 年相比，2022 年的 commit 数量增加了近一倍，这显示了巨大的进步。

同时，我们恢复了处于停止维护状态超过 10 年的[开发者网站](https://www.gimp.org/news/2022/10/16/gimp-developer-website-rewrite/)。

我们还有一个悬而未决的项目，即将主网站移植到 *Hugo* 框架。不幸的是，这不可能在 2022 年完成。

## 2023 年的计划

### GIMP 3.0.0？

我不应该给出日期，所以不要把它当作承诺。也许这只是一个愚蠢的人的愚蠢梦想：我目前正计划**在 2023 年发布 GIMP 3.0.0，或者至少发布我们的第一个候选版本。**

如上，如果没有按时发布，请记住这不是承诺。😜

还有很多事情要做，所以我希望我不是在自欺欺人。但在某些时候，无法发布只会令人沮丧。当然，我们仍在可接受的开发周期内（GIMP 2.8 到 2.10 花了 6 年；我们仍处于 2.10 以来的第 5 个年头）但我真的很想结束它。

现在为了让这个截止日期生效，我决定推迟 [3.0 路线图](https://developer.gimp.org/core/roadmap/#gimp-30)中的一些元素。特别是：

- 扩展管理：这个项目对我来说很重要，因为我开始它并开发了已经实现的东西，但要获得一个安全的在线基础设施来处理扩展搜索和下载，我们需要时间。
- 颜料选取工具：非常棒的工具，但它的贡献者 Thomas Manni 对性能不满意（它需要即时画布反馈才能使用）并且目前正在研究替代算法。

与此同时，当我意识到审查它并进行来回更正将花费我们数周的时间时，我一直在搁置一些为我们贡献的不错的新代码。例如，你们中的一些人可能已经在社交网络上看到了 Nikc（基于 Hendrik Boom 和 Jacob Boerema 的工作）漂亮的“[矢量图层](https://www.youtube.com/watch?v=7M8R_yU7RhM)”演示。该功能不会并入 GIMP 3.0。

这是我应用于我自己的代码的规则。例如，有些人可能确实记得我自己的[链路层实验](https://www.youtube.com/watch?v=N5oyqbD7zyQ)，出于同样的原因，我在 2 年前停止了这项工作。

此类事件仍然会发生，我只是将这些目标移到进一步的版本中，我将在下一节中解释。

### 重新思考路线图

这使我想到了我最近一直在做的关于我们的路线图和发布计划化的组织工作。到今天为止，你一定读过很多关于我们的双版本计划。**GIMP 3.0** 用于 GTK+3 移植，**3.2** 用于高级非破坏性编辑。

虽然这第二个目标在我们的路线图中仍然是一个很大的计划，但我不认为让它再次成为一个有几十个功能并花费数年的巨大开发周期是最明智的事情。这种旧的开发模式在过去是有意义的，但在我看来，现在已经没有意义了。

我对 GIMP 的目标是更频繁地发布，加快开发周期，也许一次发布的功能较少，但每次发布都有不错的功能。这是我一直在推动的事情，从 2014 年开始，当时我还是个新人（[在 2014 年 LGM 期间的一次会议上](https://developer.gimp.org/conferences/lgm/2014-leipzig_germany/lgm_2014_minutes/#release-management)，我第一次提出我们应该在微型版本中也能发布新功能）。这最终导致我们从 GIMP 2.10.0 开始，[改变了发布政策](https://www.gimp.org/release-notes/gimp-2.10.html#roadmap-and-whats-next)。而这也是我想继续进一步推动的事情。

所以我想说的是，在遥远的未来，以 “GIMP 3.2” 为目标，已经没有意义了。非破坏性编辑功能，如非破坏性的图层效果，将会实现，但它会出现在 GIMP 3.2.0 吗？还是某个 3.0.x 版本？我们将拭目以待。反正都是数字而已。我们最终可能会把它分解成更小的版本。

考虑到这一点，我把我们的 3.0 之后的路线图分成小块，按照我们想要的和一定会发生的项目的逻辑分类进行审查。

- 链接层和矢量图层现在属于新的“[非破坏性层类型](https://developer.gimp.org/core/roadmap/#non-destructive-layer-types)”类别。不会并入 GIMP 3.0.0 的代码是如此先进以至于它会是一种浪费，但它肯定会在发布后立即成为主要目标之一。也许在 GIMP 3.0.2 中发布？  
顺便说一句，这也为我们期待已久的形状功能打开了大门：有了矢量图层，我们可以进行非破坏性的形状绘制。我的意思是，画布上的形状应该是矢量特征才能使其正确！
- 非破坏性[图层效果](https://developer.gimp.org/core/roadmap/#non-destructive-filters)（以前是 3.2 的主要目标）显然是一个独立的项目。
- [宏](https://developer.gimp.org/core/roadmap/#macros-script-recording)支持也是我们长期以来一直想要的东西，在 GIMP 3.0 中，我们已经开始为此功能奠定基础。这有望很快成为现实。
- [动画支持](https://developer.gimp.org/core/roadmap/#animation-and-multi-page-support)，你们大多数人都知道，这是我多年来一直从事的工作，总有一天会出现在 GIMP 中。所以它也是它自己的类别。它还将带来多页面支持（不仅仅是作为页面的图层）。
- 我们的[扩展平台](https://developer.gimp.org/core/roadmap/#extensions)还在计划中！
- [Space Invasion](https://developer.gimp.org/core/roadmap/#space-invasion) 项目将继续：对于 3.0，我们专注于我们已经支持的颜色模型的正确性；3.0 之后，我们可能会进一步研究新的颜色模型后端，例如核心 CMYK 或 L*a*b* 支持，还有专色通道等等……
- 现在我们的操场上有一堆未完成的[工具](https://developer.gimp.org/core/roadmap/#tools)，如果我们花时间完成它们就好了。当然，我们也有不错的新工具的想法。最后还有一些我们真正想要改进的工具，比如我们值得更多喜爱的文本工具。
- 最后，我们开始增强“[画布](https://developer.gimp.org/core/roadmap/#canvas)”的概念，从 [GIMP 2.10.14](https://www.gimp.org/news/2019/10/31/gimp-2-10-14-released/#out-of-canvas-viewing-and-editing) 开始，它具有“显示全部”功能。我们一直想走得更远，也想重新设计图层维度的概念（例如，使用自动增长图层，甚至无限图层功能）。
- ……

这就是我完全重写路线图页面的方式。希望有些人会喜欢阅读新页面并发现它令人兴奋。 请注意，内容并没有太大变化，只是在 GIMP 3.0 发布后它被重新组织以更加强调 GIMP 演变的更大笔画，从而使当前贡献者推动 GIMP 走向的方向更加明显（希望如此）。

## 结论

这就是我们所在的位置。我预计 2023 将是重要的一年。2022 年也非常棒，但也很累，以至于有几个星期我什么都做不了，尤其是在为发布而高强度开发[^2]之后不久。我还更加注重养成更健康的工作习惯，例如使用可调节高度的办公桌（适合坐着和站着工作）和定期散步。

这也是为什么我致力于制定程序以获得更快的发布、更好的基础设施和更好的新贡献者入职文档的原因。我的目标是走一条更有条理的道路，同时保持稍微🌪️混乱❤️‍🔥的核心，这真的让我们团队的工作变得如此愉快。😊

正如我在去年的报告中所说，GIMP 不仅是一个自由软件，它还是一个社区软件：随机的人一起做一些美好的事情并与大家分享。为什么？因为我们可以，因为我们想要。这就是为什么我喜欢我们的小社区，那有恰到好处的混乱和疯狂，闪耀着恰到好处的组织。

最后别忘了您可以 💌 [赞助 GIMP 项目并亲自资助几位 GIMP 开发人员](https://www.gimp.org/donating/)，作为回馈和加速 GIMP 开发的一种方式。如您所知，**我作为 GIMP 的维护者（通过 “ZeMarmot” 项目）和 Øyvind 作为 GEGL 的维护者正在为本报告所涉及的工作进行众筹。**感谢任何支持，以帮助我们在这样的努力中取得成功。

祝大家在 2023 年和兔年🐇 幸福、快乐🥳、身体健康！

[^1]: Jehan
[^2]: 原文是：*“especially soon after coding bursts for releases”*