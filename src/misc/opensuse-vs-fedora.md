# openSUSE Tumbleweed 与 Fedora 体验上的异同

openSUSE Tumbleweed 和 Fedora KDE 我都用过一阵子。所以基于自己的使用体验，随意地聊一下两者的异同。

## 稳定性

<u>一般而言，两者的稳定度差异不大。</u>但它们遇到问题的情景会略有不同。

Tumbleweed 会在上游发生某些变更，或没有被 OpenQA 检查出来的情况下发生问题。问题产生的可能性与快照大小没有必然联系，openSUSE 每月至少会有一个大型快照。有时候大半年都没有什么问题，也有时候一个无线驱动故障拖了大半个月才修好。

Fedora 比较容易在新版本刚刚发布的时候因为测试并不充分而产生故障，新版本大概发布后一两个月就会稳定下来。

就故障发生的频次而言，Tumbleweed 会比 Fedora 更频繁一点（毕竟是滚动发行版）。

但很多 Tumbleweed 用户对此是无所畏惧的，因为有 btrfs 快照兜底，只要还能进 BIOS，大抵还是有救的。大概所有滚动发行版中也就 Tumbleweed 用户可以完全不看更新日志了。

## 更新内容

Tumbleweed 追求为用户提供最新，最稳定的软件包。Fedora 基于它使命和原则[^four]，则致力于为用户提供**版本最新且技术最新**的软件包。

[^four]: [Fedora’s Mission and Foundations](https://docs.fedoraproject.org/en-US/project/c)

这个差异在 KDE wayland 上很明显地体现出来了，Fedora KDE 是最早抛弃 X11 的发行版，而 openSUSE 至今依旧提供 X11 会话。初次启动刚安装好的系统，且没有取消自动登陆，Tumbleweed 用户就会立即进入 KDE X11 会话。

Tumbleweed 更新比 Fedora 更频繁，更新快照发布的频次为 1~2 日/次，这使得快照常常充满了各种小更新。

## 软件包

理论上，Fedora 的软件包多于 openSUSE Tumbleweed，开发者给 RPM 系统打包会优先考虑 Fedora/RHEL。

实际上，流行的软件包两者都有打包，我认识的开发者打包时多多少少也会考虑到 Tumbleweed。更有可能出现的问题是开发者只打包了 DEB，然后只能指望社区有没有人打包了 RPM、appimage 或者 Flathub。

## 管理工具

openSUSE 的 YaST 之于 sysadmin，有些类似于 ArchLinux 之于 Linuxer。它很好用，对用户友好，对新手大概是友好的吧？当然负面效果就是 openSUSE 会比 Fedora 多了一个软件包组（我有时觉得这是冗余的，因为真的很少用它）。

Fedora 就是很常规的 Linux 了，主要的管理工具都是 CLI，网上的教程也很多，比如著名的《Linux Bible》就是以和 Fedora 同源的 RHEL 作为示例系统。当然 Fedora KDE 也有预装一些图形化的软件包，比如防火墙。但总的来说，Fedora 是认为用户应使用 CLI 管理系统的。

zypper 和 dnf 选一个的话，我大概会选 dnf。虽然我更喜欢 zypper 的 TUI，但我更想要 dnf 那稳定高速的并行下载体验。

## KDE の风味

Fedora KDE 比 Tumbleweed 预装了更多的 KDE 套件，但 KDE 上留有的 Fedora 特征并不多（就换了一下壁纸，塞了两三个红帽开发的 GUI 管理工具），体验类似于无印版。

Tumbleweed KDE 留有的大蜥蜴特征就比较多，开始菜单里的 YaST 全家桶就引人注目，openSUSE 还专门做了一整套 KDE 主题。