---
comments: true
tags:
  - 新闻
  - TheEvilSkeleton
  - Flatpak
---

# 对《Developers are lazy, thus Flatpak》的回应

## 作品信息

- 原文：[Response to "Developers are lazy, thus Flatpak"](https://theevilskeleton.gitlab.io/2023/06/04/response-to-developers-are-lazy-thus-flatpak.html)
- 作者：[TheEvilSkeleton](https://theevilskeleton.gitlab.io/about)
- 许可证：[CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/legalcode)
- 译者：暮光的白杨
- 日期：2023-06-18

----

## 导言

最近，Martijn Braam 发表了一篇题为《[Developers are lazy, thus Flatpak][origin]》的文章，对 Flatpak 的一些事情进行了批评。我想回顾一下这篇文章并解决该文提出的一些问题。

[origin]: https://blog.brixit.nl/developers-are-lazy-thus-flatpak

虽然作者 Martijn 将 Flatpak 与 [Alpine Linux][Alpine] 进行了对比，但我将把 Flatpak 与流行的 Linux 发行版进行对比，因为对我来说，将 Flatpak 与一些最常用的发行版进行对比是有意义的。

[Alpine]: https://www.alpinelinux.org/

我建议在阅读本文前，先阅读他的文章，因为我不会对该文提出的每个问题都进行回复。

## “Flatpak 是一个发行版”

=== "译文" 

    > 虽然开发者喜欢假装 Flatpak 不是一个发行版，但它仍然可疑地接近于一个发行版。它缺少内核和一些服务，也缺少标准的 Linux 基本目录规范，但它仍然是你需要瞄准的发行版。它没有提供单独的软件包和包管理器，而是提供一个带有一堆依赖项的运行时。它还方便地提供多个运行时，以确保实际上没有唯一的工作基础。因为你有时需要 Gnome 库，有时需要 KDE 库。由于没有包管理器，它们将以单独的运行时的形式提供。

=== "原文"

    > While the developers like to pretend real hard that Flatpak is not a distribution, it’s still suspiciously close to one. It lacks a kernel and a few services and it lacks the standard Linux base directory specification but it’s still a distribution you need to target. Instead of providing seperate [sic] packages with a package manager it provides a runtime that comes with a bunch of dependencies. Conveniently it also provides multiple runtimes to make sure there’s not actually a single base to work on. Because sometimes you need Gnome libraries, sometimes you need KDE libraries. Since there’s no package manager those will be in seperate [sic] runtimes.

我不太确定谁否认了 Flatpak 是一个发行版，但我肯定不是其中之一。如果有的话，这对我来说似乎是一件非常好的事情，因为我们正在打包一个发行版并将其部署到用户的系统中，这意味着他们将获得与我们开发者测试的完全相同的构建。它使我们更容易排除故障，因为环境几乎相同。

引自 Flatpak 的文档：“^^Flatpak 是一个在各种 Linux 发行版上发布桌面应用程序的框架。^^”甚至 Flatpak 也同意它是一个发行版。

## “没有内置包管理器”

=== "译文"

    > 如果你需要一个不包含在运行时内的依赖项，则没有包管理器可用于引入该依赖项。解决方案是你自己打包所需的依赖项，然后让 flatpak 工具将其构建到应用程序的 flatpak 包中。因此，现在你不再是你的应用程序的开发者，你也是这个半发行版中所有依赖关系的维护者，你在一个应用程序的伪装下分发文件。

=== "原文"

    > If you need a dependency that’s not in the runtime there’s no package manager to pull in that dependency. The solution is to also package the dependencies you need yourself and let the flatpak tooling build this into the flatpak of your application. So now instead of being the developer for your application you’re also the maintainer of all the dependencies in this semi-distribution you’re shipping under the disguise of an application.

分离每个依赖项真的很不方便，就像在 Linux 发行版中管理图形应用程序一样。例如，如果一个名为 XYZ 的 flatpak 需要 10 个依赖项，而其中一个依赖项更成了 XYZ 的开发者尚未测试的版本，那么它可能会导致错误甚至损坏，这将使开发者很难追溯，因为他们需要经常联系每个打包者并一起找出答案。因此，正如作者所提到的，解决方案是让开发者捆绑所有内容，因为他们在一个更容易理解的环境中测试他们的应用程序。

分离所有的依赖关系对于较小的程序和依赖关系来说比较容易，因为它们的依赖关系量较小，因此对于故障排除没有那么困难。然而，Flatpak 的目标是图形应用程序，这些应用程序通常很大且复杂，并且需要一个可以很好地扩展的模型。在这种情况下，让开发者管理依赖关系是一种更好的方法，因为它更容易维护和排除故障[^1]。

[^1]: 除非协作和创建 BaseApps 更实用。 BaseApps 的用例是打包框架等所需的依赖项，以减少重复工作量。

=== "译文"

    > 有一件事是肯定的，我不相信应用程序开发者会维护依赖关系。

=== "原文"

    > And one thing is for sure, I don’t trust application developers to maintain dependencies.

这是完全公平的，因为你有权发表自己的意见。同样，我也不相信在大型发行版中打包依赖关系的大量非程序员/开发者，尤其是当他们没有开发应用程序的实际经验时，这一点令人担忧。此外，我更不相信那些允许依赖关系自毁以[防止出错][wrong]的软件包管理器。

[wrong]: https://memoryfile.codeberg.page/posts/Distribution-packaging-for-Linux-desktop-applications-is-unsustainable/#steam-on-pop_os

=== "译文"

    >通过查看一些多媒体软件，这真的很疯狂。让我们看看 Audacity flatpak。构建它所需的依赖项有：
    >
    >- wxwidgets
    >- ffmpeg
    >- sqlite
    >- chrpath
    >- portaudio
    >- portmidi
    >
    >因此，让我们看看这里的依赖关系管理得如何。由于我们现在几乎正好是 2023 年的半年，我将查看过去 6 个月的更新并将其与 Alpine Linux 中的相同依赖项进行比较。
    >
    >- audacity 在 flatpak 中已经更新了 4 次。 它已经在 Alpine 上更新了 5 次。
    >- ffmpeg 已经在 flatpak 和 Alpine 中更新到 6.0，但是 ffmpeg 包有 9 个更新，因为编解码器已经更新。
    >- sqlite 在 flatpak 中没有更新，在 Alpine 中更新了 4 次。
    >- wxwidgets 在 flatpak 中没有更新，在 Alpine 中更新了 2 次。
    >- chrpath 没有更新。
    >- portaudio 没有更新。
    >- portmidi 没有更新。
    >
    >这只是我随机挑选的一个包，它已经比 flatpak 拥有更多的依赖维护。这很可能无法扩展到让所有开发者跟踪所有软件的所有依赖项。

=== "原文"

    >This gets really nuts by looking at some software that deals with multimedia. Lets [sic] look at the Audacity flatpak. It builds as dependency:
    >
    >- wxwidgets
    >- ffmpeg
    >- sqlite
    >- chrpath
    >- portaudio
    >- portmidi
    >
    >So lets [sic] look at how well dependencies are managed here. Since we’re now almost exactly half a year into 2023 I’ll look at the updates for the last 6 months and compare it to the same dependencies in Alpine Linux.
    >
    >- audacity has been updated 4 times in the flatpak. It has been updated 5 times on Alpine.
    >- ffmpeg has been updated to 6.0 in both the flatpak and Alpine, but the ffmpeg package has had 9 updates because if [sic] codecs that have been updated.
    >- sqlite hasn’t been updated in the flatpak and has been updated 4 times in Alpine
    >- wxwidgets hasn’t been updated in the flatpak and has been updated 2 times in Alpine
    >- chrpath hasn’t had updates
    >- portaudio hasn’t had updates in flatpak and Alpine.
    >- portmidi hasn’t had updates
    >
    >This is just a random package I picked and it already had a lot more maintainance of the dependencies than the flatpak has. It most likely doesn’t scale to have all developers keep track of all the dependencies of all their software.

这里的主要问题是 Audacity 有很多技术限制。举个例子，在 [Audacity 的源代码][audacity]中，有这样的记载：“^^**Linux** 上的 Audacity 使用原始版本的 wxWidgets，我们**要求**使用 **3.1.3** 版本。这个版本在大多数发行版中都不可用。^^”

[audacity]: https://github.com/audacity/audacity/blob/d895438a7a153b30cf167e6889f171394cdf7fe1/BUILDING.md

这就是 Audacity flatpak 中的 wxWidgets 没有更新的原因。如果 wxWidgets 更新主要版本，那么他们可能会遇到问题。另一方面，Alpine Linux 打包了 [wxWidgets 3.2.2.1]，如上所示，它在“必需”版本之外。

[wxWidgets 3.2.2.1]: https://web.archive.org/web/20230604034343/https://pkgs.alpinelinux.org/packages?name=wxwidgets&branch=edge&repo=&arch=&maintainer=

即便如此，在这种情况下，更新的数量并不表示哪个维护得更好，因为环境绝对重要。最好的办法是向 flatpak 的维护者咨询他们的决定，而不是在没有进行彻底调查的情况下挑选其他依赖并误导自己。

## “隔离的想法”

此部分整个都在批评 GNOME Software，而不是 Flatpak 本身。GNOME Software 是一个前端，它决定了他们应该如何向用户显示 flatpaks。如果 GNOME 软件开发者愿意，他们可以在用户尝试安装应用程序时显示一个警告对话框。在这种情况下，他们反对它并将其放在一个单独的按钮下。在我看来，当前端“错误”地做事时，责怪后端程序是不公平的[^2]。

[^2]: 以我的拙见，我更喜欢 GNOME Software 的方法，因为它的阻碍更少，也不会妨碍我。

## “那么传统发行版呢”

=== "译文"

    >我听过很多用户和开发者对 Flatpak 的争论，但最后我很难说利大于弊。
    >
    >我认为开发者没有权限在安全系统的伪装下将他们想要的任何代码推送给每个人，这一点非常重要。这就是我作为软件开发者的看法。
    >
    >发行版打包的软件至少有一定程度的审查，并且通常至少会确保构建标志被设置为禁用用户跟踪和此类功能。

=== "原文"

    >I’ve heard many argument for Flatpaks by users and developers but in the end I can’t really say the pros outweigh the cons.
    >
    >I think it’s very important that developers do not have the permissions to push whatever code they want to everyone under the disguise of a secure system. And that’s my opinion as a software developer.
    >
    >Software packaged by distributions has at least some degree of scrutiny and it often results in at least making sure build flags are set to disable user tracking and such features.

正如用户 [u/natermer] 在 Reddit 上解释得非常好：“^^发行版打包的审查和安全优势被许多捍卫它的人严重夸大了。只有少数高知名度的软件包接受过严格的安全审查。在大多数情况下，打包者只需要做足够的工作，让软件正确构建，并在不破坏任何东西的情况下安装依赖关系，然后由最终用户报告任何出现的问题。这意味着此类保证并不真正比 ‘pip’ 之类的东西好多少。^^”

[u/natermer]: https://www.reddit.com/r/linux/comments/yzvqry/in_defense_of_traditional_packaging/ix2evov/?context=3

在上面讨论的 Audacity 示例中，我提到 Alpine Linux 将 [wxWidgets 3.2.2.1] 和 Audacity 打包在一起，Audacity 已明确声明使用 3.1.3 版本。这是打包作者仅投入足够的工作来构建软件的一个很好的例子。应用程序启动不足以断定它运行良好。虽然作者的观点是关于安全的，但我认为安全之外的审查同样重要，应该被提及。

此外，这不仅仅是审查的问题，还有维护的问题，或缺乏维护的问题。让我们看看一些流行的发行版在其仓库中包含的软件包的数量。截止到写这篇文章时：

- Debian 有超过 [1,200 个孤立的软件包][debian]（没有维护者），其中许多已经超过 1,000 天了！
- Fedora Linux 有超过 [800 个孤立的软件包][fedora]（没有维护者）。
- Arch Linux 有超过 [400 个标记为过时的软件包][arch]。

[debian]: https://web.archive.org/web/20230425093359/https://www.debian.org/devel/wnpp/orphaned
[fedora]: https://packager-dashboard.fedoraproject.org/dashboard?users=orphan
[arch]: https://web.archive.org/web/20230604023515/https://archlinux.org/packages/?sort=&q=&maintainer=&flagged=Flagged

因此，虽然作者担心开发人员会“错误处理”与 Flatpak 的依赖关系，但我们观察到更令人担忧的是你在系统上运行的发行版中有大量未维护的软件包；这些软件包安装在**你的主机**上。因此，你要么选择在你的主机上有一个处理不当的软件包，要么选择在容器内有一个处理不当的依赖关系。我很乐意选择后者。

=== "译文"

    >我也相信，如果软件在制作过程中希望能在 Flatpak 之外运行，那么一般来说会更好。确保在不需要的情况下不依赖最新版本的库并不难。在软件中拥有可选的依赖关系并不难。实际上遵循 XDG 规范而不是硬编码路径并不难。

=== "原文"

    >I also believe software in general is better if it’s made with the expectation that it will run outside of Flatpak. It’s not that hard to make sure you don’t depend on bleeding edge versions of libraries while that’s not needed. It’s not that hard to have optional dependencies in software. It’s not that hard to actually follow XDG specifications instead of hardcoding paths.

我曾经写过《[Traditional Packaging is not Suitable for Modern Applications][2022-08]》，其中深入探讨了缺乏强大的依赖管理的问题。总而言之，在传统的打包系统中，开发者无法挑选依赖项，如果不利用相同的容器或类似的容器，发行版就无法提供一致的体验和环境。

[2022-08]: https://theevilskeleton.gitlab.io/2022/08/29/traditional-packaging-is-not-suitable-for-modern-applications.html

以 Audacity 为例，在传统的打包系统中，如果发行版选择使用 wxWidgets 的最新版本，即 [3.2.2.1]，并且发行版发布了该版本，那么打包人员就不能特意使用 3.1.3 或其他版本，否则该版本将与现有版本冲突。

[3.2.2.1]: https://github.com/wxWidgets/wxWidgets/releases/tag/v3.2.2.1

这不仅仅是关于版本的问题，而且还包括应用自定义补丁。例如，从我的文章中可以看出，[OBS Studio 需要一些补丁][obs]来使 ffmpeg 与 OBS Studio 整齐地整合在一起。如果发行版提供的 ffmpeg 没有打过补丁，或者没有 OBS Studio 要求的补丁，就无法做到这一点，否则他们就得寻找各种变通办法（参考我的文章）。

[obs]: https://github.com/obsproject/obs-studio/blob/fe889ec28ebd2f323b5933b7b11c5a9207539c59/CI/flatpak/com.obsproject.Studio.json#L259-L261

传统的打包系统不能解决的另一个问题是提供一个一致和可预测的环境。例如，[Bottles] 是一个“脆弱”的软件，因为它需要 Wine 和其他应用的环境，以便在安全、封闭和可预测的环境中运行。我已经写了一篇[很长的评论][comment]，解释了为什么支持传统的打包系统对我们来说是一种负担，而且充其量是不可行的。

[comment]: https://github.com/bottlesdevs/Bottles/issues/2345#issuecomment-1574284415
[Bottles]: https://flathub.org/apps/com.usebottles.bottles

另一个例子是 Steam 使用的 [steam-runtime-tools]，它使用 [bubblewrap]，一个起源于 Flatpak 的工具，来包含和隔离游戏。即使 Steam 支持许多发行版，但客户端都源自同一个存档，这意味它们都是相同的二进制文件，因此在某种程度上是一致的，就像 Flatpak 一样。

[steam-runtime-tools]: https://gitlab.steamos.cloud/steamrt/steam-runtime-tools
[bubblewrap]: https://github.com/containers/bubblewrap

正如 [Linus Torvalds] 所说：“^^……我向你保证 Valve 不会制作 15 种不同的二进制文件……^^”——在现实中，有其他事情要担心的开发者不可能花时间为 5 个不同的 Linux 发行版构建他们的软件并持续支持它们。公平地说，[“5” 是一种轻描淡写的说法][5]。相反，他们只想要一个二进制文件，并不断地对该二进制文件进行彻底测试，然后将其发送给用户。

[Linus Torvalds]: https://youtu.be/Pzl1B7nB9Kc?t=328
[5]: https://upload.wikimedia.org/wikipedia/commons/8/8c/Linux_Distribution_Timeline_Dec._2020.svg

## “但为发行版打包是困难的”

=== "译文"

    >这是最好的事情！开发者不应该是打包软件的人，所以一点也不难。让你的软件进入所有发行版不是你的任务，如果你的软件对人们有用，往往有人主动打包。我有一些软件在 Alpine Linux、ALT Linux、Archlinux AUR、Debian、Devuan、Fedora、Gentoo、Kali、LiGurOS、Nix、OpenMandriva、postmarketOS、Raspbian、Rosa、Trisquel、Ubuntu 和 Void 被打包好了。我不必打包其中的大部分内容。
    >
    >我从其他打包我的软件的发行版中注意到的最多的是来自维护者的补丁，这些补丁改进了软件，通常是在处理一些边缘情况时我忘记了某处的硬编码路径。
    >
    >实际上，我在发行版打包上花费时间最多的是我成功推送到 Flathub 的几款软件。处理发行版之间的差异很容易，处理在 Flatpak 内部和外部运行之间的差异很难。

=== "原文"

    >That’s the best thing! Developers are not supposed to be the ones packaging software so it’s not hard at all. It’s not your task to get your software in all the distributions, if your software is useful to people it tends to get pulled in. I have software that’s packaged in Alpine Linux, ALT Linux, Archlinux AUR, Debian, Devuan, Fedora, Gentoo, Kali, LiGurOS, Nix, OpenMandriva, postmarketOS, Raspbian, Rosa, Trisquel, Ubuntu and Void. I did not have to package most of this.
    >
    >The most I notice from other distributions packaging my software is patches from maintainers that improve the software, usually in dealing with some edge case I forgot with a hardcoded path somewhere.
    >
    >The most time I’ve ever spent on distribution packaging is actually the few pieces of software I’ve managed to push to Flathub. Dealing with differences between distributions is easy, dealing with differences between runing [sic] inside and outside Flatpak is hard.

撇开受虐狂不谈，正如作者在文章开头所写，“[s]adly there's reality”。现实情况是，开发人员不会也无法处理发行版造成的所有负担。他们想要一些易于维护的东西，同时可以他们认为合适的方式，控制打包他们想要的一切东西，因为开发者知道他们的程序是如何工作的。他们可以在环境中测试他们的应用程序，并将同样的环境发送给用户。

如果发行版不努力让开发人员更轻松地打包、测试并将他们的应用程序交付给用户，那么发行版就无法吸引大多数开发人员。Flatpak 和 Flathub 打包难度的降低正是许多发行版开始默认包含和使用 Flathub 的原因，例如 SteamOS；这就是为什么 GNOME 和 KDE 一直将 Flatpak 作为主要分发方法的原因；这也是 Flatpak 和 Flathub 迅速流行起来的原因。

## “但 Flatpaks 对最终用户来说更简单”

=== "译文"

    >我在 Pinebook Pro 上遇到的第二个问题是，它有一个 64GB 的 rootfs。使用太多的 flatpak 是非常浪费空间的。理论上，你有一个具有主要依赖项的运行时，然后 flatpak 仅有数兆字节的内容。实际上，我几乎为每个已安装的 flatpak 都安装了一个独特的平台，因为 flatpak 依赖于该平台的不同版本或仅依赖于不同的平台。

=== "原文"

    >A second issue I’ve had on my Pinebook Pro is that it has a 64GB rootfs. Using too many flatpaks is just very wasteful of space. In theory you have a runtime that has your major dependencies and then a few Megabytes of stuff in your application flatpak. In practice I nearly have an [sic] unique platform per flatpak installed because the flatpaks depend on different versions of that platform or just on different platforms.

虽然我有一个 256 GB 的 SSD，但我可能比普通用户拥有更多的图形应用程序，因为我测试并贡献了几个应用程序。我甚至可以说我的 Flatpak 设置是被诅咒的，但我离题了。

许多应用程序使用不同的运行时和不同版本的运行时。我相信我属于“每个安装的 flatpak 我几乎都有一个独特的平台”类别，尽管我相信他的说法是夸张的。

首先，我们将检查所有 flatpaks 占用的存储量。我将使用 [flatpak-dedup-checker] 来测量存储使用情况：

[flatpak-dedup-checker]: https://gitlab.com/TheEvilSkeleton/flatpak-dedup-checker

```
$ ./flatpak-dedup-checker --user
Directories:                /var/home/TheEvilSkeleton/.local/share/flatpak/{runtime,app}
Size without deduplication: 89.31 GB
Size with deduplication:    37.73 GB (42% of 89.31 GB)
Size with compression:      27.66 GB (30% of 89.31 GB; 73% of 37.73 GB)
```

我们注意到所有没有压缩的 flatpak 总共占用 37.73 GB。让我们看看我安装了多少应用程序：

```
$ flatpak list --app --user | wc -l
173
```

173 个**图形**应用程序——包括主要浏览器，例如 Firefox、LibreWolf、Tor Browser、Chromium、ungoogled-chromium、Google Chrome、Brave、Microsoft Edge 和 Epiphany。如果你对此好奇，请查看下表：

```
$ flatpak list --app --user
Name                                         Application ID                                                Version                                Branch                    Origin
Dialect                                      app.drey.Dialect                                              2.1.1                                  stable                    flathub
Elastic                                      app.drey.Elastic                                              0.1.3                                  stable                    flathub
Cambalache                                   ar.xjuan.Cambalache                                           0.10.3                                 stable                    flathub
Valent                                       ca.andyholmes.Valent                                                                                 master                    valent-origin
Dconf Editor                                 ca.desrt.dconf-editor                                         43.0                                   stable                    flathub
Decoder                                      com.belmoussaoui.Decoder                                      0.3.3                                  stable                    flathub
ASHPD Demo                                   com.belmoussaoui.ashpd.demo                                   0.3.0                                  stable                    flathub
Bitwarden                                    com.bitwarden.desktop                                         2023.5.0                               stable                    flathub
Boxy SVG                                     com.boxy_svg.BoxySVG                                          3.96.0                                 stable                    flathub
Brave Browser                                com.brave.Browser                                             1.52.117                               stable                    flathub
Tally for Plausible                          com.cassidyjames.plausible                                    3.0.1                                  stable                    flathub
Discord Canary                               com.discordapp.DiscordCanary                                  0.0.156                                beta                      flathub-beta
Mindustry                                    com.github.Anuken.Mindustry                                   144.3                                  stable                    flathub
ungoogled-chromium                           com.github.Eloston.UngoogledChromium                          113.0.5672.126                         stable                    flathub
Gradience                                    com.github.GradienceTeam.Gradience                            0.4.1                                  stable                    flathub
Desktop Files Creator                        com.github.alexkdeveloper.desktop-files-creator               1.2.2                                  stable                    flathub
Eyedropper                                   com.github.finefindus.eyedropper                              0.6.0                                  stable                    flathub
Rnote                                        com.github.flxzt.rnote                                        0.6.0                                  stable                    flathub
Wike                                         com.github.hugolabe.Wike                                      2.0.1                                  stable                    flathub
Text Pieces                                  com.github.liferooter.textpieces                              3.4.1                                  stable                    flathub
Tor Browser Launcher                         com.github.micahflee.torbrowser-launcher                      0.3.6                                  stable                    flathub
G4Music                                      com.github.neithern.g4music                                   1.13                                   stable                    flathub
Czkawka                                      com.github.qarmin.czkawka                                     5.1.0                                  stable                    flathub
Clapper                                      com.github.rafostar.Clapper                                   0.5.2                                  stable                    flathub
Logisim-evolution                            com.github.reds.LogisimEvolution                              3.8.0                                  stable                    flathub
Avvie                                        com.github.taiko2k.avvie                                      2.3                                    stable                    flathub
Flatseal                                     com.github.tchx84.Flatseal                                    2.0.1                                  stable                    flathub
Frog                                         com.github.tenderowl.frog                                     1.3.0                                  stable                    flathub
Video Downloader                             com.github.unrud.VideoDownloader                              0.12.4                                 stable                    flathub
Easy Effects                                 com.github.wwmm.easyeffects                                   7.0.4                                  stable                    flathub
NewsFlash                                    com.gitlab.newsflash                                          2.3.0                                  stable                    flathub
Google Chrome                                com.google.Chrome                                             113.0.5672.126-1                       stable                    flathub
Inochi Creator                               com.inochi2d.inochi-creator                                   0.8.0                                  stable                    flathub
qView                                        com.interversehq.qView                                        5.0                                    stable                    flathub
GtkStressTesting                             com.leinardi.gst                                              0.7.5                                  stable                    flathub
Forge Sparks                                 com.mardojai.ForgeSparks                                      0.1.1                                  stable                    flathub
Extension Manager                            com.mattjakeman.ExtensionManager                              0.4.0                                  stable                    flathub
Microsoft Edge                               com.microsoft.Edge                                            114.0.1823.37-1                        stable                    flathub
OBS Studio                                   com.obsproject.Studio                                         29.1.2                                 stable                    flathub
Share Preview                                com.rafaelmardojai.SharePreview                               0.3.0                                  stable                    flathub
Black Box                                    com.raggesilver.BlackBox                                      0.13.2                                 stable                    flathub
Geopard                                      com.ranfdev.Geopard                                           1.4.0                                  stable                    flathub
Transmission                                 com.transmissionbt.Transmission                               4.0.3                                  stable                    flathub
Bottles                                      com.usebottles.bottles                                        51.6                                   master                    bottles-origin
Bottles                                      com.usebottles.bottles                                        51.6                                   stable                    flathub
Steam                                        com.valvesoftware.Steam                                       1.0.0.75                               stable                    flathub
Visual Studio Code                           com.visualstudio.code                                         1.78.2-1683731010                      stable                    flathub
Fragments                                    de.haeckerfelix.Fragments                                     2.1.1                                  stable                    flathub
Tubefeeder                                   de.schmidhuberj.tubefeeder                                    v1.9.4                                 master                    tubefeeder-origin
Tubefeeder                                   de.schmidhuberj.tubefeeder                                    v1.9.6                                 stable                    flathub
Tuba                                         dev.geopjr.Tuba                                               0.3.2                                  stable                    flathub
Forecast                                     dev.salaniLeo.forecast                                        0.1.0                                  stable                    flathub
HandBrake                                    fr.handbrake.ghb                                              1.6.1                                  stable                    flathub
Metadata Cleaner                             fr.romainvigier.MetadataCleaner                               2.5.2                                  stable                    flathub
Cartridges                                   hu.kramo.Cartridges                                           1.5.4                                  stable                    flathub
Element                                      im.riot.Riot                                                  1.11.31                                stable                    flathub
Cinny                                        in.cinny.Cinny                                                2.2.6                                  stable                    flathub
Komikku                                      info.febvre.Komikku                                           1.21.1                                 stable                    flathub
Amberol                                      io.bassi.Amberol                                              0.10.3                                 stable                    flathub
Bavarder                                     io.github.Bavarder.Bavarder                                   0.2.3                                  stable                    flathub
AdwSteamGtk                                  io.github.Foldex.AdwSteamGtk                                  0.6.0                                  stable                    flathub
Youtube Downloader Plus                      io.github.aandrew_me.ytdn                                     3.14.0                                 stable                    flathub
Epic Asset Manager                           io.github.achetagames.epic_asset_manager                      3.8.4                                  stable                    flathub
GPU-Viewer                                   io.github.arunsivaramanneo.GPUViewer                          2.26                                   stable                    flathub
Celluloid                                    io.github.celluloid_player.Celluloid                          0.25                                   stable                    flathub
Escambo                                      io.github.cleomenezesjr.Escambo                               0.1.1                                  stable                    flathub
PinApp                                       io.github.fabrialberio.pinapp                                 1.1.7                                  stable                    flathub
Monitorets                                   io.github.jorchube.monitorets                                 0.10.0                                 stable                    flathub
AppImage Pool                                io.github.prateekmedia.appimagepool                           5.1.0                                  stable                    flathub
Kooha                                        io.github.seadve.Kooha                                        2.2.3                                  stable                    flathub
Mousai                                       io.github.seadve.Mousai                                       0.7.5                                  stable                    flathub
WebCord                                      io.github.spacingbat3.webcord                                 4.2.0                                  stable                    flathub
Converter                                    io.gitlab.adhami3310.Converter                                1.6.1                                  stable                    flathub
Sudoku Solver                                io.gitlab.cyberphantom52.sudoku_solver                        1.0.1                                  stable                    flathub
Letterpress                                  io.gitlab.gregorni.ASCIIImages                                1.3.0                                  stable                    flathub
Calligraphy                                  io.gitlab.gregorni.Calligraphy                                1.0.0                                  stable                    flathub
LibreWolf                                    io.gitlab.librewolf-community                                 113.0.2-1                              stable                    flathub
Upscaler                                     io.gitlab.theevilskeleton.Upscaler                                                                   master                    upscaler3-origin
Upscaler                                     io.gitlab.theevilskeleton.Upscaler                            1.1.2                                  stable                    flathub
Upscaler                                     io.gitlab.theevilskeleton.Upscaler                            1.1.2                                  test                      upscaler1-origin
Devel                                        io.gitlab.theevilskeleton.Upscaler.Devel                                                             master                    devel-origin
Dev Toolbox                                  me.iepure.devtoolbox                                          1.0.2                                  stable                    flathub
Passes                                       me.sanchezrodriguez.passes                                    0.7                                    stable                    flathub
Lutris                                       net.lutris.Lutris                                             0.5.13                                 stable                    flathub
Mullvad Browser                              net.mullvad.MullvadBrowser                                    102.9.0esr-12.0-2-build1               stable                    flathub
Poedit                                       net.poedit.Poedit                                             3.3.1                                  stable                    flathub
RPCS3                                        net.rpcs3.RPCS3                                               0.0.28-1-33558d14                      stable                    flathub
Live Captions                                net.sapples.LiveCaptions                                      0.4.0                                  stable                    flathub
Color Picker                                 nl.hjdskes.gcolor3                                            2.4.0                                  stable                    flathub
Audacity                                     org.audacityteam.Audacity                                     3.3.2                                  stable                    flathub
Chromium Web Browser                         org.chromium.Chromium                                         114.0.5735.90                          stable                    flathub
Chromium application base                    org.chromium.Chromium.BaseApp                                                                        21.08                     flathub
Electron2 application base                   org.electronjs.Electron2.BaseApp                                                                     21.08                     flathub
Electron2 application base                   org.electronjs.Electron2.BaseApp                                                                     22.08                     flathub
Flatpak External Data Checker                org.flathub.flatpak-external-data-checker                                                            stable                    flathub
Builder                                      org.flatpak.Builder                                                                                  stable                    flathub
Piper                                        org.freedesktop.Piper                                         0.7                                    stable                    flathub
VulkanInfo                                   org.freedesktop.Platform.VulkanInfo                                                                  22.08                     flathub
appstream-glib                               org.freedesktop.appstream-glib                                0.8.1                                  stable                    flathub
Feeds                                        org.gabmus.gfeeds                                             2.2.0                                  stable                    flathub
Giara                                        org.gabmus.giara                                              1.1.0                                  stable                    flathub
Zola                                         org.getzola.zola                                              0.17.2                                 stable                    flathub
GNU Image Manipulation Program               org.gimp.GIMP                                                 2.99.14                                beta                      flathub-beta
GNU Image Manipulation Program               org.gimp.GIMP                                                 2.10.34                                master                    gnome-nightly
GNU Image Manipulation Program               org.gimp.GIMP                                                 2.10.34                                stable                    flathub
Adwaita Demo                                 org.gnome.Adwaita1.Demo                                       1.4.alpha                              master                    gnome-nightly
Boxes                                        org.gnome.Boxes                                               44.2                                   stable                    flathub
Builder                                      org.gnome.Builder                                             44.2                                   stable                    flathub
Calculator                                   org.gnome.Calculator                                          44.0                                   stable                    flathub
Calendar                                     org.gnome.Calendar                                            44.0                                   stable                    flathub
Contacts                                     org.gnome.Contacts                                            44.0                                   stable                    flathub
Devhelp                                      org.gnome.Devhelp                                             43.0                                   stable                    flathub
Web                                          org.gnome.Epiphany                                            44.3                                   stable                    flathub
File Roller                                  org.gnome.FileRoller                                          43.0                                   stable                    flathub
Firmware                                     org.gnome.Firmware                                            43.2                                   stable                    flathub
Fractal                                      org.gnome.Fractal.Devel                                       5~beta1-c3d77b7                        master                    gnome-nightly
Geary                                        org.gnome.Geary                                               43.0                                   stable                    flathub
Glade                                        org.gnome.Glade                                               3.40.0                                 stable                    flathub
Lollypop                                     org.gnome.Lollypop                                            1.4.37                                 stable                    flathub
Loupe                                        org.gnome.Loupe                                               44.3                                   stable                    flathub
Maps                                         org.gnome.Maps                                                44.2                                   stable                    flathub
Files                                        org.gnome.NautilusDevel                                       44.1                                   master                    gnome-nightly
Notes                                        org.gnome.Notes                                               40.1                                   stable                    flathub
Document Scanner                             org.gnome.SimpleScan                                          44.0                                   stable                    flathub
Text Editor                                  org.gnome.TextEditor                                          44.0                                   stable                    flathub
Endeavour                                    org.gnome.Todo                                                43.0                                   stable                    flathub
Videos                                       org.gnome.Totem                                               43.0                                   stable                    flathub
Weather                                      org.gnome.Weather                                             44.0                                   stable                    flathub
Pika Backup                                  org.gnome.World.PikaBackup                                    0.6.1                                  stable                    flathub
Secrets                                      org.gnome.World.Secrets                                       7.3                                    stable                    flathub
Clocks                                       org.gnome.clocks                                              44.0                                   stable                    flathub
App Icon Preview                             org.gnome.design.AppIconPreview                               3.3.0                                  stable                    flathub
Contrast                                     org.gnome.design.Contrast                                     0.0.8                                  stable                    flathub
Emblem                                       org.gnome.design.Emblem                                       1.2.0                                  stable                    flathub
Icon Library                                 org.gnome.design.IconLibrary                                  0.0.16                                 stable                    flathub
Lorem                                        org.gnome.design.Lorem                                        1.2                                    stable                    flathub
Color Palette                                org.gnome.design.Palette                                      2.0.2                                  stable                    flathub
Symbolic Preview                             org.gnome.design.SymbolicPreview                              0.0.8                                  stable                    flathub
Typography                                   org.gnome.design.Typography                                   0.2.0                                  stable                    flathub
Fonts                                        org.gnome.font-viewer                                         44.0                                   stable                    flathub
Identity                                     org.gnome.gitlab.YaLTeR.Identity                              0.5.0                                  stable                    flathub
Iotas                                        org.gnome.gitlab.cheywood.Iotas                               0.1.16                                 stable                    flathub
Apostrophe                                   org.gnome.gitlab.somas.Apostrophe                             2.6.3                                  stable                    flathub
GTK Demo                                     org.gtk.Demo4                                                                                        master                    gnome-nightly
Inkscape                                     org.inkscape.Inkscape                                         1.2.2                                  stable                    flathub
JDownloader                                  org.jdownloader.JDownloader                                   2.0                                    stable                    flathub
Dolphin                                      org.kde.dolphin                                               23.04.1                                stable                    flathub
Kdenlive                                     org.kde.kdenlive                                              23.04.1                                stable                    flathub
Krita                                        org.kde.krita                                                 5.1.5                                  stable                    flathub
NeoChat                                      org.kde.neochat                                               23.04.1                                stable                    flathub
Xwayland Video Bridge                        org.kde.xwaylandvideobridge                                                                          master                    xwaylandvideobridge-origin
KeePassXC                                    org.keepassxc.KeePassXC                                       2.7.5                                  stable                    flathub
LibreOffice                                  org.libreoffice.LibreOffice                                   7.5.3.2                                stable                    flathub
Thunderbird                                  org.mozilla.Thunderbird                                       102.11.2                               stable                    flathub
Firefox                                      org.mozilla.firefox                                           113.0.2                                stable                    flathub
Tagger                                       org.nickvision.tagger                                         2022.11.2                              stable                    flathub
Nicotine+                                    org.nicotine_plus.Nicotine                                    3.2.9                                  stable                    flathub
ONLYOFFICE Desktop Editors                   org.onlyoffice.desktopeditors                                 7.3.3                                  stable                    flathub
Helvum                                       org.pipewire.Helvum                                           0.4.0                                  stable                    flathub
qBittorrent                                  org.qbittorrent.qBittorrent                                   4.5.3                                  stable                    flathub
Tenacity                                     org.tenacityaudio.Tenacity                                    1.3-beta3                              test                      tenacity-origin
Wine                                         org.winehq.Wine                                               7.0                                    stable-21.08              flathub
Wine                                         org.winehq.Wine                                               8.0                                    stable-22.08              flathub
Zrythm                                       org.zrythm.Zrythm                                             1.0.0-beta.4.9.1                       stable                    flathub
Imaginer                                     page.codeberg.Imaginer.Imaginer                               0.2.2                                  stable                    flathub
Atoms                                        pm.mirko.Atoms                                                1.1.1                                  stable                    flathub
Commit                                       re.sonny.Commit                                               4.0                                    stable                    flathub
Oh My SVG                                    re.sonny.OhMySVG                                              1.2                                    stable                    flathub
Playhouse                                    re.sonny.Playhouse                                            1.1                                    stable                    flathub
Workbench                                    re.sonny.Workbench                                            44.1                                   stable                    flathub
Graphs                                       se.sjoerd.Graphs                                              1.5.2                                  stable                    flathub
Cawbird                                      uk.co.ibboard.cawbird                                         1.5                                    stable                    flathub
ArmCord                                      xyz.armcord.ArmCord                                           3.2.0                                  stable                    flathub
```

好吧，让我们看看安装的运行时数量：

```
$ flatpak list --runtime --user | wc -l
97
```

以及运行时本身：

```
$ flatpak list --runtime --user
Name                                                     Application ID                                                 Version                                      Branch                 Origin
Codecs                                                   com.github.Eloston.UngoogledChromium.Codecs                                                                 stable                 flathub
Proton (community build)                                 com.valvesoftware.Steam.CompatibilityTool.Proton               7.0-6                                        beta                   flathub-beta
Proton (community build)                                 com.valvesoftware.Steam.CompatibilityTool.Proton               7.0-5                                        stable                 flathub
Proton experimental (community build)                    com.valvesoftware.Steam.CompatibilityTool.Proton-Exp           7.0-20230208                                 stable                 flathub
Proton-GE (community build)                              com.valvesoftware.Steam.CompatibilityTool.Proton-GE            8.3-1                                        beta                   flathub-beta
Proton-GE (community build)                              com.valvesoftware.Steam.CompatibilityTool.Proton-GE            8.3-1                                        stable                 flathub
gamescope                                                com.valvesoftware.Steam.Utility.gamescope                      3.11.51                                      stable                 flathub
Codecs                                                   org.audacityteam.Audacity.Codecs                                                                            stable                 flathub
Codecs                                                   org.chromium.Chromium.Codecs                                                                                stable                 flathub
Calf                                                     org.freedesktop.LinuxAudio.Plugins.Calf                        0.90.3                                       22.08                  flathub
LSP                                                      org.freedesktop.LinuxAudio.Plugins.LSP                         1.2.6                                        22.08                  flathub
MDA                                                      org.freedesktop.LinuxAudio.Plugins.MDA                         1.2.10                                       22.08                  flathub
TAP-plugins                                              org.freedesktop.LinuxAudio.Plugins.TAP                         1.0.1                                        22.08                  flathub
ZamPlugins                                               org.freedesktop.LinuxAudio.Plugins.ZamPlugins                  4.1                                          22.08                  flathub
SWH                                                      org.freedesktop.LinuxAudio.Plugins.swh                         0.4.17                                       22.08                  flathub
Freedesktop Platform                                     org.freedesktop.Platform                                       21.08.18                                     21.08                  flathub
Freedesktop Platform                                     org.freedesktop.Platform                                       22.08.12.1                                   22.08                  flathub
i386                                                     org.freedesktop.Platform.Compat.i386                                                                        21.08                  flathub
i386                                                     org.freedesktop.Platform.Compat.i386                                                                        22.08                  flathub
Mesa                                                     org.freedesktop.Platform.GL.default                            21.3.9                                       21.08                  flathub
Mesa                                                     org.freedesktop.Platform.GL.default                            23.1.1                                       22.08                  flathub
Mesa (Extra)                                             org.freedesktop.Platform.GL.default                            23.1.1                                       22.08-extra            flathub
Mesa git snapshot                                        org.freedesktop.Platform.GL.mesa-git                           23.0-branchpoint-4408-g4ac56e3e5a4           23.08beta              flathub-beta
default                                                  org.freedesktop.Platform.GL32.default                                                                       21.08                  flathub
Mesa                                                     org.freedesktop.Platform.GL32.default                          23.1.1                                       22.08                  flathub
Mesa (Extra)                                             org.freedesktop.Platform.GL32.default                          23.1.1                                       22.08-extra            flathub
Mesa git snapshot                                        org.freedesktop.Platform.GL32.mesa-git                         23.0-branchpoint-4408-g4ac56e3e5a4           23.08beta              flathub-beta
MangoHud                                                 org.freedesktop.Platform.VulkanLayer.MangoHud                  0.6.9-1                                      22.08                  flathub
vkBasalt                                                 org.freedesktop.Platform.VulkanLayer.vkBasalt                  0.3.2.9                                      22.08                  flathub
ffmpeg-full                                              org.freedesktop.Platform.ffmpeg-full                                                                        21.08                  flathub
ffmpeg-full                                              org.freedesktop.Platform.ffmpeg-full                                                                        22.08                  flathub
i386                                                     org.freedesktop.Platform.ffmpeg_full.i386                                                                   21.08                  flathub
i386                                                     org.freedesktop.Platform.ffmpeg_full.i386                                                                   22.08                  flathub
openh264                                                 org.freedesktop.Platform.openh264                              2.1.0                                        2.0                    flathub
openh264                                                 org.freedesktop.Platform.openh264                              2.1.0                                        2.0beta                flathub-beta
openh264                                                 org.freedesktop.Platform.openh264                              2.1.0                                        2.2.0                  flathub
openh264                                                 org.freedesktop.Platform.openh264                              2.1.0                                        2.2.0beta              gnome-nightly
Freedesktop SDK                                          org.freedesktop.Sdk                                            21.08.18                                     21.08                  flathub
Freedesktop SDK                                          org.freedesktop.Sdk                                            22.08.12.1                                   22.08                  flathub
i386                                                     org.freedesktop.Sdk.Compat.i386                                                                             21.08                  flathub
i386                                                     org.freedesktop.Sdk.Compat.i386                                                                             22.08                  flathub
.NET Core SDK extension                                  org.freedesktop.Sdk.Extension.dotnet6                          6.0.408                                      21.08                  flathub
Free Pascal Compiler and Lazarus                         org.freedesktop.Sdk.Extension.freepascal                       3.2.2                                        21.08                  flathub
Go programming language Sdk extension                    org.freedesktop.Sdk.Extension.golang                           1.20.2                                       21.08                  flathub
OpenJDK 11 SDK Extension                                 org.freedesktop.Sdk.Extension.openjdk11                                                                     21.08                  flathub
OpenJDK 17 SDK Extension                                 org.freedesktop.Sdk.Extension.openjdk17                                                                     22.08                  flathub
Rust stable                                              org.freedesktop.Sdk.Extension.rust-stable                      1.67.0                                       21.08                  flathub
Rust stable                                              org.freedesktop.Sdk.Extension.rust-stable                      1.70.0                                       22.08                  flathub
toolchain-i386                                           org.freedesktop.Sdk.Extension.toolchain-i386                                                                21.08                  flathub
toolchain-i386                                           org.freedesktop.Sdk.Extension.toolchain-i386                                                                22.08                  flathub
toolchain-i386                                           org.freedesktop.Sdk.Extension.toolchain-i386                                                                22.08beta              flathub-beta
GNOME Boxes Osinfo DB                                    org.gnome.Boxes.Extension.OsinfoDb                             20230518                                     stable                 flathub
HEIC                                                     org.gnome.Loupe.HEIC                                                                                        stable                 flathub
GNOME Application Platform version 41                    org.gnome.Platform                                                                                          41                     flathub
GNOME Application Platform version 42                    org.gnome.Platform                                                                                          42                     flathub
GNOME Application Platform version 43                    org.gnome.Platform                                                                                          43                     flathub
GNOME Application Platform version 44                    org.gnome.Platform                                                                                          44                     flathub
GNOME Application Platform version Nightly               org.gnome.Platform                                                                                          master                 gnome-nightly
i386                                                     org.gnome.Platform.Compat.i386                                                                              41                     flathub
i386                                                     org.gnome.Platform.Compat.i386                                                                              43                     flathub
i386                                                     org.gnome.Platform.Compat.i386                                                                              44                     flathub
GNOME Software Development Kit version 41                org.gnome.Sdk                                                                                               41                     flathub
GNOME Software Development Kit version 42                org.gnome.Sdk                                                                                               42                     flathub
GNOME Software Development Kit version 43                org.gnome.Sdk                                                                                               43                     flathub
GNOME Software Development Kit version 44                org.gnome.Sdk                                                                                               44                     flathub
GNOME Software Development Kit version Nightly           org.gnome.Sdk                                                                                               master                 gnome-nightly
i386                                                     org.gnome.Sdk.Compat.i386                                                                                   41                     flathub
i386                                                     org.gnome.Sdk.Compat.i386                                                                                   42                     flathub
i386                                                     org.gnome.Sdk.Compat.i386                                                                                   43                     flathub
i386                                                     org.gnome.Sdk.Compat.i386                                                                                   44                     flathub
i386                                                     org.gnome.Sdk.Compat.i386                                                                                   master                 gnome-nightly
Codecs                                                   org.gnome.Totem.Codecs                                                                                      stable                 flathub
yt-dl totem-pl-parser plugin                             org.gnome.Totem.Videosite.YouTubeDl                                                                         stable                 flathub
Adwaita dark GTK theme                                   org.gtk.Gtk3theme.Adwaita-dark                                                                              3.22                   flathub
adw-gtk3 Gtk Theme                                       org.gtk.Gtk3theme.adw-gtk3                                                                                  3.22                   flathub
adw-gtk3-dark Gtk Theme                                  org.gtk.Gtk3theme.adw-gtk3-dark                                                                             3.22                   flathub
Adwaita theme                                            org.kde.KStyle.Adwaita                                                                                      6.4                    flathub
Kvantum theme engine                                     org.kde.KStyle.Kvantum                                         1.0.6                                        5.15-21.08             flathub
KDE Application Platform                                 org.kde.Platform                                                                                            5.15-21.08             flathub
KDE Application Platform                                 org.kde.Platform                                                                                            5.15-22.08             flathub
KDE Application Platform                                 org.kde.Platform                                                                                            6.4                    flathub
QGnomePlatform                                           org.kde.PlatformTheme.QGnomePlatform                                                                        5.15-21.08             flathub
QGnomePlatform                                           org.kde.PlatformTheme.QGnomePlatform                                                                        5.15-22.08             flathub
QGnomePlatform                                           org.kde.PlatformTheme.QGnomePlatform                                                                        6.4                    flathub
QtSNI                                                    org.kde.PlatformTheme.QtSNI                                                                                 5.15-21.08             flathub
KDE Software Development Kit                             org.kde.Sdk                                                                                                 5.15-21.08             flathub
KDE Software Development Kit                             org.kde.Sdk                                                                                                 6.4                    flathub
QGnomePlatform-decoration                                org.kde.WaylandDecoration.QGnomePlatform-decoration                                                         5.15-21.08             flathub
QGnomePlatform-decoration                                org.kde.WaylandDecoration.QGnomePlatform-decoration                                                         5.15-22.08             flathub
QGnomePlatform-decoration                                org.kde.WaylandDecoration.QGnomePlatform-decoration                                                         6.4                    flathub
Codecs                                                   org.kde.krita.Codecs                                                                                        stable                 flathub
DXVK                                                     org.winehq.Wine.DLLs.dxvk                                      1.10.3                                       stable-21.08           flathub
DXVK                                                     org.winehq.Wine.DLLs.dxvk                                      1.10.3                                       stable-22.08           flathub
Gecko                                                    org.winehq.Wine.gecko                                                                                       stable-21.08           flathub
Gecko                                                    org.winehq.Wine.gecko                                                                                       stable-22.08           flathub
Mono                                                     org.winehq.Wine.mono                                                                                        stable-21.08           flathub
Mono                                                     org.winehq.Wine.mono                                                                                        stable-22.08           flathub
```

在该输出中，这里有一些有趣的部分：

```
Name                                                     Application ID                                                 Version                                      Branch                 Origin

[…]

GNOME Application Platform version 41                    org.gnome.Platform                                                                                          41                     flathub
GNOME Application Platform version 42                    org.gnome.Platform                                                                                          42                     flathub
GNOME Application Platform version 43                    org.gnome.Platform                                                                                          43                     flathub
GNOME Application Platform version 44                    org.gnome.Platform                                                                                          44                     flathub
GNOME Application Platform version Nightly               org.gnome.Platform                                                                                          master                 gnome-nightly

[…]

KDE Application Platform                                 org.kde.Platform                                                                                            5.15-21.08             flathub
KDE Application Platform                                 org.kde.Platform                                                                                            5.15-22.08             flathub
KDE Application Platform                                 org.kde.Platform                                                                                            6.4                    flathub

[…]

DXVK                                                     org.winehq.Wine.DLLs.dxvk                                      1.10.3                                       stable-21.08           flathub
DXVK                                                     org.winehq.Wine.DLLs.dxvk                                      1.10.3                                       stable-22.08           flathub
Gecko                                                    org.winehq.Wine.gecko                                                                                       stable-21.08           flathub
Gecko                                                    org.winehq.Wine.gecko                                                                                       stable-22.08           flathub
Mono                                                     org.winehq.Wine.mono                                                                                        stable-21.08           flathub
Mono                                                     org.winehq.Wine.mono                                                                                        stable-22.08           flathub
```

我们可以观察到，就像作者一样，我有很多不同版本的运行时。即使有异常数量的运行时和应用程序，Flatpak 以某种方式设法使用 37.73GB，即使大多数浏览器都安装为 flatpak。

我想大多数用户都安装了一小部分应用程序，这些应用程序只有几个运行时和一个版本，这也意味着我的设置可能是最糟糕的情况之一。即使有那么多的折磨，Flatpak 仍然设法很好地处理存储。

## “Flatpak 确实有它的用途”

=== "译文"

    >我不会说 Flatpak 是完全无用的。对于某些使用情况，有它的存在是很好的。我认为 Flatpak 在需要发布闭源软件的时候最有意义。

=== "原文"

    >I wouldn’t say Flatpak is completely useless. For certain usecases [sic] it is great to have available. It [sic] think Flatpak makes most sense for when closed source software would need to be distributed.

这是一个真正需要解决的问题：Flatpak（Flathub）对于自由和开源（FOSS）的应用程序来说甚至比闭源**更有**意义，因为它们使应用程序容易被人们所知。例如，[Upscaler]，一个被我作为 [CS50x] 的期末作业开发的应用程序，于 [2022 年 11 月 16 日]发布在 Flathub 上，同日[^mouse]被 [OMG! Ubuntu! 进行了报道][omgubunt]。另一个例子，[gregorni]，我的一个朋友，在 [2023 年 6 月 1 日]发表了 [Calligraphy]，[同一天在 OMG! Linux! 上刊登][omglinux]。

[Upscaler]: https://flathub.org/apps/io.gitlab.theevilskeleton.Upscaler
[CS50x]: https://www.edx.org/course/introduction-computer-science-harvardx-cs50x
[2022 年 11 月 16 日]: https://github.com/flathub/flathub/pull/3648#issuecomment-1316659408
[omgubunt]: https://www.omgubuntu.co.uk/2022/11/upscaler-open-source-ai-image-upscale-app-for-linux
[gregorni]: https://gitlab.gnome.org/gregorni
[Calligraphy]: https://flathub.org/apps/io.gitlab.gregorni.Calligraphy
[2023 年 6 月 1 日]: https://github.com/flathub/flathub/pull/4194#issuecomment-1571390015
[omglinux]: https://www.omglinux.com/calligraphy-ascii-text-art-linux

[^mouse]: 你可以将鼠标悬停在新闻的日期上查看发布日期

虽然在 Flathub 上发布闭源应用程序很有意义，但在我看来，FOSS 应用程序比闭源应用程序获得更多好处，因为新闻媒体，尤其是针对 FOSS 的新闻媒体，会很快发现你的应用程序，甚至可能会写一篇关于它们的文章。这也意味着更多用户会发现你的应用程序，这有助于应用越来越受欢迎。这也意味着你不必依赖 GitHub 来让你的应用程序被发现。如果你的应用程序发布在专为被发现而设计的商店中，你实际上可以使用 [Codeberg] 并让它们很容易被发现。

[Imaginer] 和 [Bavarder] 是一些主要在 Codeberg 上可用的 GTK4+libadwaita 应用程序，但它们平均每天都获得超过 100 次下载，这在我看来是一个相当大的成就。

[Codeberg]: https://codeberg.org/
[Imaginer]: https://codeberg.org/Imaginer/Imaginer
[Bavarder]: https://codeberg.org/Bavarder/Bavarder

## 结论

最后，我尊重不喜欢 Flatpak 的意见，因为我们都有不同的看法，在很多事情上意见不一。但是，发表意见与被误导并将其展示给观众或读者之间是有区别的，尤其是当它可能会伤害正在努力解决 Linux 桌面上的真正问题的社区时。

作为应用程序开发人员，我无法预测用户的设置；我不想浪费时间研究 Fedora Linux、Debian、Ubuntu、Arch Linux 等软件包的依赖项及其版本。我不想浪费我的时间将用户推荐给 Bugzilla、邮件列表、IRC 或其他没有人愿意使用的不便平台，然后发现他们已经打包了 6 个月前的应用程序版本。相反，我更愿意把我的时间花在修复实际的错误、添加新功能、调整一些设计、从事其他项目、写文章，或者，你知道的，来一次户外旅行。