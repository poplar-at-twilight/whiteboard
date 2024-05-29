---
comments: true
tags:
  - 新闻
  - GIMP
---

# 基于 AppImage 的新 GIMP 测试工具

- 译文信息：
    - 源文：[Experiments with AppImage](https://www.gimp.org/news/2024/05/28/experiments-appimage/)
    - 作者：[Bruno](https://www.gimp.org/author/bruno.html)
    - 许可证：[CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
    - 日期：2024-05-29
    - 译者：暮光的白杨

----

今年四月和五月初，我们一直在幕后改进我们持续集成（CI）和与编译相关的代码。在此过程中，我们遇到一个问题：如何在 Linux 容易地测试合并请求（merge request）？例如，在 Windows 上，我们的每次提交（commit）都有一个 `.zip` 文件；在 macOS 上，我们有 `.app` 文件（位于 `.dmg` 文件内）。对于 Linux……我们之前什么都没有（虽然有每周的 Flatpak 构建，但用于测试目的却非常耗时）。经过简短的讨论，我们决定采用 AppImage 格式。

[AppImage] 是一个应用程序包格式，本质上是一个捆绑包（bundle），非常适合上述的开发和测试工作流。需要明确的是，⚠️ **我们目前还没有将 AppImage 发布为官方软件包** ⚠️（本文稍后会详细介绍）。关于这些实验……

[AppImage]: https://appimage.org/

## 选择“正确”的工具

AppImage 没有强制性的 SDK[^mean1]。创建过程可以使用一些免费的工具完成，例如 `linuxdeployqt`、`appimage-builder` 和 `AppImageKit`。不过我们选择使用 `go-appimage`，因为它用途广泛，而且我们只需要进行快速测试构建。

[^mean1]: [Software development kit][sdk]

[sdk]: https://en.wikipedia.org/wiki/Software_development_kit

在我们的例子中，该工具负责打包几乎所有依赖项，并通过恰当的 ELF[^mean2] 数据将所有内容压缩成一个可单击执行的文件。但是，该工具并没有原生支持猜测不同软件的细节（例如脚本解释器的使用），因此我们需要[手动]复制和设置一些东西。顺便说一句，我们已经在 `go-appimage` 代码仓库中提交了一些 issue，希望能改进一些功能，这也是[自由与开源软件协作]的一个小例子。

[^mean2]: [Executable and Linkable Format][ELF]

[手动]: https://gitlab.gnome.org/GNOME/gimp/-/raw/master/build/linux/appimage/bundle-gimp-appimage.sh
[ELF]: https://en.wikipedia.org/wiki/Executable_and_Linkable_Format
[自由与开源软件协作]: https://github.com/probonopd/go-appimage/issues/282

## 从过往的 appimage 学习

当然，我们并不是从头开始的！我们借鉴了其他非官方的 GIMP AppImage 构建版本（你可以在此处找到相关[列表]）。也许还存在其他版本，但我们只找到了这四个。

[列表]: https://gitlab.gnome.org/GNOME/gimp/-/raw/master/build/linux/appimage/AppRun

我们还联系了这些非官方构建版本的开发者，就潜在的官方 AppImage 进行测试和反馈。非常感谢他们！此外，其他人也做出了贡献（你可以在此[合并请求]中找到有关信息）。感谢所有参与人员！😄

[合并请求]: https://gitlab.gnome.org/GNOME/gimp/-/merge_requests/1440

## 将 Wilber 的智慧修补到 appimage 中

我们不能简单地将这些非官方 appimages 代码放入我们的存储库中，因为软件不是这样工作的。

我们的代码（甚至包括打包代码）都先于新的打包方式（在本例中是捆绑（bundling））存在，因此在将这些代码放入新的打包方式之前，我们需要考虑到这些现有代码并进行相应的调整。

考虑到我们过去的打包代码和经验，我们定义了一些在批准新的打包格式之前要检查的原则。简而言之，该格式需要：

1. *其脚本需要位于 GIMP 代码仓库中，并使用 GIMP/GNOME runners 以提高透明度（macOS 目前例外，这是历史遗留问题，理想情况下应该在未来得到修复）；*
1. *其脚本基于官方 GIMP git 源代码/二进制文件进行构建/打包，以提高安全性；*
1. *其脚本应尽可能简化并保持可读性，以方便维护。*

最后一项原则是为将来有人接手维护这个软件包而准备的，这也是我们目前无法正式发布 AppImage 的主要原因。目前尚且没有遵守这些原则的人志愿承担维护 appimage 的责任。因此，我们能做的最好的事情就是遵循这些原则，制作一个测试用的捆绑包。

## 实际使用和未来

我们的 AppImage 处于可用状态，并确实用于处理问题并测试合并请求，使用范围是 master 分支支持的 Debian 版本（目前为 Debian 12）。这确实非常方便，下面我们来解释一下原因：

假设你没有一台性能强劲的机器，这种情况很普遍。通常情况下，你只会为特定的平台原生构建 GIMP 并进行贡献，因为在虚拟机中进行构建会非常繁琐。但是，得益于我们的测试 AppImage：

* Windows 用户只需登录虚拟机，从 MR 下载 Debian artifact 并进行测试即可。我们有使用 Windows VM 的贡献者（他们可以下载跨系统的（cross） `.zip` artifact），现在也可以进行反向操作了。
* 这对于处理问题也很有用：在分类问题时会推荐最新的 master 分支版本，因此需要不断进行本地重新构建。幸运的是，由于我们的持续集成会为每次新的提交自动生成一个 AppImage，所以这不再是必需的操作。

当然，这仅仅是一个有限的用例，这使得我们的 AppImage 并不适合在开发者版本下载页面进行链接。并不是每个贡献者都使用 Debian 12 系统，也并非每个 Windows 或 Mac 用户都拥有一个 Debian 12 的虚拟机。坦白地说，如果该 AppImage 出现一些我们无法修复的问题，那么它随时都可能被放弃。

因此，**我们欢迎为提高与其他发行版**（至少是最旧的受支持的 Ubuntu 和最新的 Fedora）**的兼容性做出贡献**。如果你对此感兴趣，[请与我们联系]。

[请与我们联系]: https://www.gimp.org/discuss.html