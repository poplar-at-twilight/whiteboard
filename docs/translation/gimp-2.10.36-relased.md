---
comments: true
tags:
  - 新闻
  - GIMP
---

# GIMP 2.10.36 已发布!

- 译文信息：
    - 源文：[GIMP 2.10.36 Released](https://www.gimp.org/news/2023/11/07/gimp-2-10-36-released/)
    - 作者：[Jehan](https://www.gimp.org/author/jehan.html)
    - 许可证：[CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
    - 日期：2023-11-07
    - 译者：暮光的白杨
    - 补档翻译日期：2024-02-14

----

这个稳定版的 GIMP 附带了一些安全修复，因此即使你觉得当前老版本运行良好，我们也建议你进行更新。除了许多错误修复和安全更新外，它还提供了对调色板格式的新支持和新生成的渐变效果。

*本新闻列出了最显著、最明显的更改。我们不会在此列出错误修复或较小的改进。要获得更完整的变更列表，请参阅[新闻文件]或查看[提交历史]。*

[新闻文件]: https://gitlab.gnome.org/GNOME/gimp/-/blob/70ad9853928ed7887df3aaaa3b607d6b646c51ed/NEWS#L11
[提交历史]: https://gitlab.gnome.org/GNOME/gimp/-/commits/gimp-2-10

## 新功能和改进

### ASE 和 ACB 调色板支持

除了已经支持的调色板格式之外，GIMP 现在还可以加载以下格式的调色板：

- **[Adobe Swatch Exchange] (ASE)**
- **[Adobe Color Book] (ACB)**

[Adobe Swatch Exchange]: https://helpx.adobe.com/indesign/using/swatches.html
[Adobe Color Book]: https://creativecloud.adobe.com/discover/article/adobe-coloring-book

这样可以更方便地交换来自其他软件的调色板。

### 新渐变：FG 到透明（硬边）

现在，只要有渐变选项，渐变列表中就会多出一个 “FG 到透明（硬边）（`FG to Transparent (Hardedge)`）” 选项。它可以生成从前景色到透明色的渐变，两种颜色之间有硬边过渡。

特别是在渐变工具中，使用“重复”选项可以快速生成图案，在给定背景上交替出现完全透明的重复彩色图形。

<iframe title="New &quot;FG to Transparent (Hardedge)&quot; gradient - GIMP 2.10.36" width="560" height="315" src="https://peer.tube/videos/embed/92f104c1-51c3-436c-bf82-4c9673edc357" frameborder="0" allowfullscreen="" sandbox="allow-same-origin allow-scripts allow-popups"></iframe>

### GIF：非平方比率支持

GIMP 现在通过为每个维度设置不同的分辨率来加载包含 `PixelAspectRatio` 标头元数据的 GIF 图像，从而正确渲染图像（而不是在屏幕上看起来被压扁）。

当然，你必须取消选中 `视图` 菜单中的“点对点”选项才能按预期比例查看图像。

### 更多增强功能

此更新还进行了一些其他改进，例如：

- 文本工具：改进了在画布上选择和更改文本时的格式化行为。
- 主题：悬停锁定按钮（带有白色边框）和激活锁定（角落里显示一个小挂锁）时提供更好的反馈。 
- 帮助：`帮助 > 用户手册` 子菜单现在提供了 “\[目录]” 链接。

## 安全与错误修复

### 修复漏洞

[Zero Day Initiative] 以以下格式的代码报告了四个漏洞并立即得到修复：

- DDS: ZDI-CAN-22093
- PSD: ZDI-CAN-22094
- PSP: ZDI-CAN-22096 和 ZDI-CAN-22097

[Zero Day Initiative]: https://www.zerodayinitiative.com/

此外，我们的二进制包中的依赖项也已更新，并且修复了这些库中最近报告的一些漏洞。

无论如何，我们建议使用最新的软件包更新 GIMP。

### 使用最新版 linuxwacom 驱动程序的绘图板出现故障

我们通常不会在显著位置提及错误修复，但最近在 `xf86-input-wacom` (`linuxwacom`) 驱动程序发生变化后，发生了一件令人难堪的事，它导致在 Linux 上使用绘图板时 GIMP 崩溃。

由于该驱动程序的补丁也已迅速推送，因此各种发行版都已将驱动程序降级或回传修复程序。不过，如果你不幸使用了未打补丁的驱动程序，这个版本的 GIMP 也包含了解决该错误的方法。

## 发布统计数据

自 GIMP 2.10.34 起：

- 26 份报告已于 2036 年 10 月 2 日作为 “FIXED（已修复）” 关闭。
- 合并了 10 个合并请求。
- 已推送 155 项提交。
- 更新了 20 个翻译：白俄罗斯语、加泰罗尼亚语、中文（中国）、丹麦语、荷兰语、格鲁吉亚语、德语、希腊语、匈牙利语、冰岛语、意大利语、立陶宛语、波兰语、葡萄牙语、罗马尼亚语、斯洛文尼亚语、西班牙语、瑞典语、土耳其语、乌克兰语。

29 人对 GIMP 2.10.36 代码库做出了更改或修复（顺序由提交数量决定）：

- 7 名开发人员：Alx Sa、Jehan、Stanislav Grinkov、Jacob Boerema、Daniel Novomeský、Andras Timar 和 Gabriel Scherer。
- 22 位译者：Marco Ciampa、Sabri Ünal、Luming Zh、Anders Jonsson、Yuri Chornoivan、Martin、Rodrigo Lledó、Balázs Úr、Hugo Carvalho、Jürgen Benvenuti、Nathan Follens、Piotr Drąg、Alan Mortensen、Cristian Secară、Ekaterine Papava、Jordi Mas、 Vasil Pupkin、Aurimas Černius、Danial Behzadi、Petr Kovář、Sveinn í Felli 和 dimspingos。
- 3 名资源创建者（图标、主题、光标、启动画面、元数据……）：Stanislav Grinkov、Jehan 和 Daniel Novomeský。
- 1 位文档贡献者：Jehan。
- 3 位构建或 CI 贡献者：Jernej Simončič、Jehan 和 Stanislav Grinkov。

对 GIMPverse 中其他存储库的贡献（顺序由提交数量决定）：

- [babl]、[GEGL] 和 [ctx] 正在积极开发中，但该版本的 GIMP 还没有发布。因此，我们将在下一个版本中提供相关统计数据。
- 自 2.10.34 发布以来，`gimp-macos-build`（macOS 构建脚本）的 `gimp-2-10` 分支有 45 次提交，提交者为 1 人： Lukas Oberhuber。
- 自 2.10.34 发布以来，稳定的 flatpak 分支有 28 次提交，由 3 位贡献者（和一个机器人）完成： Jehan、Daniel Novomeský 和 Hubert Figuière。
- 我们的[主网站]自 2.99.16 发布以来有 165 次提交，由 6 位贡献者完成： Sabri Ünal、Jehan、Bruno Lopes、lillolollo、Alx Sa 和 Robin Swift。
- 自 2.99.16 发布以来，我们的[开发者网站]有 5 位贡献者提交了 17 次内容：Jehan、Bruno Lopes、Aryeom、Jacob Boerema 和 Robin Swift。
- 自 2.10.34 版发布以来，我们的 [2.10 文档]有 16 位贡献者提交了 138 次内容：Andre Klapper、Jacob Boerema、Marco Ciampa、Anders Jonsson、Boyuan Yang、dimspingos、Yuri Chornoivan、Jordi Mas、Rodrigo Lledó、Martin、Alexander Shopov、Alx Sa、Balázs Úr、Piotr Drąg、Sabri Ünal 和 Tim Sabsch。

[babl]: https://www.gegl.org/babl/
[gegl]: https://www.gegl.org/
[ctx]: https://ctx.graphics/
[主网站]: https://www.gimp.org
[开发者网站]: https://developer.gimp.org/
[2.10 文档]: https://docs.gimp.org/

让我们不要忘记感谢所有帮助我们在 Gitlab 中分流、报告错误并与我们讨论可能的改进的人们。我们的社区也非常感谢管理我们各种[讨论频道]或社交网络账户的互联网战士，如 Ville Pätsi、Liam Quin、Michael Schumacher 和 Sevenix！

[讨论频道]: https://www.gimp.org/discuss.html

*注意：考虑到 GIMP 及其周围的部分数量，以及我们如何通过 `git` 脚本获取统计数据，这些统计数据中可能会出现错误。如果我们遗漏或错误分类了一些贡献者或贡献，请随时告诉我们。*

## 团队新闻及发布流程

最近，Lukas Oberhuber（我们的 macOS 软件包维护者）获得了 `git` 存储库的访问权限。

在 GSoC 期间，我们的 Gitlab 项目的 “reporter（报告者）” 权利被授予了 GSoC 的两位贡献者：Idriss 和 Shubham（第三位已经拥有 git 访问权限）。

[GSoC]: ./gimp-in-gsoc-2023.md

曾为 GIMP 的[开发者网站]提供过帮助的 Robin Swift 现在已经开始将[主网站]从 [Pelican] 移植到 [Hugo]，这是一个计划已久但迄今为止已陷入停滞的项目。

[Pelican]: https://getpelican.com/
[Hugo]: https://gohugo.io/

最后，我们提醒大家，我们[正在积极寻找能帮助我们在软件包发布前进行测试的人员][seek]（尤其是 GIMP 3.0 及以后的版本）。这将有助于使 GIMP 版本更加强大。自上次发布以来，Anders Jonsson 和 Mark Sweeney 已成为 Flatpak 测试人员。我们还有几位 Windows 软件包的测试员，但仍没有 MacOS 的测试员。无论您在哪个操作系统和架构上进行测试，我们都欢迎您提供反馈，以便及早发现问题！团结一致，社区更强大！💪

[seek]: https://www.gimp.org/news/2023/07/09/gimp-2-99-16-released/#team-news-and-release-process

## GIMP 的周边

### 镜像站新闻

自我们最新消息发布以来，4 个新镜像开始分发 GIMP：

- *[Silicon Hill]*，捷克共和国布拉格捷克技术大学学生俱乐部；
- *[Lancaster-Lebanon IU13]*，该组织由美国宾夕法尼亚州兰开斯特市的 20 多个公立学区和几所非公立学校、教会学校和特许学校组成；
- 位于摩洛哥拉巴特的 *[摩洛哥学术和研究广域网][MARWAN]* (MARWAN)；
- 日本东京的 [Jing Luo]。

[Silicon Hill]: https://www.siliconhill.cz/
[Lancaster-Lebanon IU13]: https://www.iu13.org/
[MARWAN]: https://www.marwan.ma/index.php/en/
[Jing Luo]: https://jing.rocks/about/

到目前为止，我们共有来自世界各地的 45 个镜像。

镜像非常重要，因为它们可以帮助项目分担每天数以万计的下载量。此外，通过遍布全球的镜像，我们确保每个人都能快速下载访问 GIMP。

### 图书新闻

Sabri Ünal 继续他们的📚书目研究，添加了许多已出版的书籍，因此我们决定将书籍完全重组为结构化文件数据库，使我们能够轻松处理信息或与数据分开更改页面样式。

这也促使我们将书籍页面分成 2 个：

- [最近关于 GIMP 的书籍](https://www.gimp.org/books/)
- [以前关于 GIMP 的书籍](https://www.gimp.org/books/older.html)

由于书籍描述并不总是清楚地说明所涉及的 GIMP 版本，因此我们使用 GIMP 2.10.0 的发布日期（2018 年 4 月 27 日）作为分割日期。

最后同样重要的是，这种新结构可以让我们轻松生成统计数据，现在我们在图书页面的底部显示了这些数据。在 GIMP 2.10.0 发布之后至少出版了 44 本图书，而在此之前出版了 305 本。因此，我们目前总共列出了 349 本关于 GIMP 的书籍，包括 17 种语言！

我们提醒大家，我们欢迎新书加入。如果你知道（甚至是作者）一本尚未列入的 GIMP 相关书籍，[请报告与列表中其他书籍相同的信息][report-book]。谢谢！

[report-book]: https://gitlab.gnome.org/Infrastructure/gimp-web/-/issues/new

## 下载 GIMP 2.10.36

你可以在 [GIMP 官方网站 (gimp.org)][主网站] 上找到我们所有的官方版本：

- 适用于 x86 和 ARM（64 位）的 Linux Flatpak
- 适用于 x86（32 和 64 位）和 ARM（64 位）的通用 Windows 安装程序
- 适用于 Intel 硬件的 macOS DMG 软件包
- 适用于 Apple Silicon 硬件的 macOS DMG 包

~~注意：*macOS 软件包有点晚了，但很快就会到来。*~~

第三方制作的其他软件包显然也会跟进（Linux 或 *BSD 发行版软件包等）。

## 下一步行动

我相信这可能是 2.10 分支的倒数第二个版本，当然这还有待确认。现实生活中可能发生的事情并不总是与计划一致。

与此同时，我们正以前所未有的努力发布 GIMP 3.0。您很快就会在我们的下一个开发版本中听到相关消息。

别忘了，你还可以[为 GIMP 开发人员捐款和提供个人资助][donating]，**以此回馈项目并加速 GIMP 的开发**。社区承诺有助于项目的发展壮大！💪🥳

[donating]: https://www.gimp.org/donating/