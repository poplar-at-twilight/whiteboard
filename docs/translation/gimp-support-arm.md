---
comments: true
tags:
  - 新闻
  - GIMP
---

# GIMP 现可在 Windows on ARM 上运行（实验性）

- 译文信息：
    - 原作者：[Jehan](https://www.gimp.org/author/jehan.html)
    - 原文：[GIMP now on Windows for ARM (experimental)](https://www.gimp.org/news/2023/08/13/experimental-windows-arm-installer/)
    - 许可证：[CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
    - 译者：暮光的白杨
    - 日期：2023-08-14

----

随着架构平台使用范围的扩大，Windows on ARM（64 位）现已成为热门话题。因此，我们决定在 Windows on ARM 上实验性地支持 GIMP！

在新发布的修订版 2 中，我们的 Windows 版 GIMP 2.10.34 通用安装程序（如我们的[下载页面]所示）将自动检测运行平台并在相关时安装 ARM 版本。

[下载页面]: https://www.gimp.org/downloads/

特别感谢我们的 Windows 打包者 Jernej Simončič 的持续工作！

## 未来工作

这种新支持的 “**实验性**” 限定理由如下：

1. 它没有经过广泛的测试。我们已经意识到一些问题，并希望发布此实验版本将帮助我们获得更多反馈。
2. 到目前为止，只有 Jernej 拥有一台 Windows on ARM 设备。尤其是据我们所知，没有一个开发人员拥有这样的硬件。因此，我们无法像其他支持平台那样快速修复 Windows/ARM 的问题。
3. 最后，但并非最不重要的一点是，我们的持续集成平台还没有设置这种额外的构建，这意味着我们无法像其他架构那样彻底、快速地发现出现的问题，也无法像我们希望的那样自动构建。

## 你可以如何提供帮助

除了[报告]和[补丁]之外，我们确实需要在持续集成平台中安装 Windows/ARM 机器。这确实是一个障碍，可能会导致我们在发布 GIMP 3 时放弃实验，因为我们不想走回头路，回到由单个贡献者在个人计算机上手动构建 3.0 系列的状态。

这意味着我们正在寻找愿意帮助我们设置 Windows on ARM 机器并将其配置为 [Gitlab 项目] runner 的人。

由于明显的安全要求，这样的志愿者需要有系统管理经验，愿意长期投入（我们可不能把有漏洞的 Windows 机器留在互联网上），并有一定的自由和开放源码软件贡献经验。

与其他跨平台自由软件项目协调以分担 CI runner 的管理负担也可能很有趣，我们可以一起使用它来为 Windows/ARM 构建 GIMP。

如果你感兴趣，请通过 [IRC] 或[专用报告]与我们联系。

[报告]: https://gitlab.gnome.org/GNOME/gimp/-/issues
[补丁]: https://gitlab.gnome.org/GNOME/gimp/-/merge_requests
[IRC]: https://www.gimp.org/discuss.html#irc-matrix
[专用报告]: https://gitlab.gnome.org/GNOME/gimp/-/issues/9170
[Gitlab 项目]: https://gitlab.gnome.org/GNOME/gimp/