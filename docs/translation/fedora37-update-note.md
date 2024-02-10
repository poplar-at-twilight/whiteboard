---
comments: true
tags:
  - 新闻
  - Fedora
  - GNOME
---

# Fedora Linux 37 更新

## 作品信息

- 原文：[Fedora Linux 37 update](https://fedoramagazine.org/fedora-linux-37-update/)
- 作者：[Ben Cotton](https://fedoramagazine.org/author/bcotton/)
- 许可证：[CC-BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/)
- 译者：暮光的白杨
- 日期：2022-10-28

----

![01](./images/2022-10/f37-release-1024x433.webp)

Fedora Linux 37 要推迟发布；要很晚了。究其原因，你可能已经听说过，[OpenSSL 项目宣布](https://mta.openssl.org/pipermail/openssl-announce/2022-October/000238.html)了一个将于周二发布，针对一个致命（[`critical-severity`](https://www.browserstack.com/guide/bug-severity-vs-priority) 级别，可导致系统直接宕机）错误的修复版本。在周二发布之前，我们不会知道这个问题的细节，但它可能很重要。因此，我们决定推迟发布 Fedora Linux 37。[我们现在的目标是在 11 月 15 日发布](https://fedorapeople.org/groups/schedule/f-37/f-37-key-tasks.html)。

## 不完整的信息

大多数决策都是在信息不完整的情况下发生的。这一次特别不完整。如果您不熟悉[禁运流程](https://www.redhat.com/en/blog/security-embargoes-red-hat)，你可能不明白为什么。当发现安全问题时，这个信息通常会被保密地与项目分享。这允许开发人员在更多人知道并利用它之前解决问题。然后，项目与下游共享信息，以便他们做好准备。

具有讽刺意味的是，Fedora 的开放性意味着我们无法提前开始准备。我们所有的构建管道和工件都是开放的。如果我们要开始构建更新，这将在禁运解除之前披露漏洞。因此，我们只知道 OpenSSL 认为这是最高级别的严重性，Red Hat 的产品安全团队强烈建议我们在发布 Fedora Linux 37 之前等待修复。

## 平衡时间与质量

作为 Fedora 项目经理，[我们](https://getfedora.org/)的发布计划是[我](https://fedoramagazine.org/author/bcotton/)的责任。我为从前任那里继承的准时发布记录感到自豪。我们把此纪录保持到了 2021 年 4 月的 Fedora Linux 34。在那段时间里，我们进行了重大的技术更改（比如切换到 Btrfs 作为大多数[变体](https://spins.fedoraproject.org/en/)的默认设置）并彼此共同经历一场[大流行](https://en.wikipedia.org/wiki/Covid-19)。我为社区在困难的情况下能够取得的成就感到自豪。

但准时并不是唯一的因素。我们知道你依赖 Fedora Linux 工作和娱乐，因此质量始终是一个考虑因素。知道我们因为 OpenSSL 漏洞要延迟时，问题变成了“推迟多长时间”？

我们在星期四基于预期于周二发布的安全修复版本，做出“发布/不发布”决定。这为镜像文件更新到镜像站提供了时间。OpenSSL 项目团队计划在我们为 11 月 8 日的 Fedora 37 做出“发布/不发布”决定之前 48 小时内发布安全修复程序。考虑到构建更新的 openssl 包并生成候选版本的时间，这给了我们大约一天半的时间来进行测试。我们没有足够的时间来适应对如此重要的软件包的更改。

因此，我们额外给自己留下一周的时间，这样我们就可以确信 Fedora Linux 37 具有你所期望的相同质量水平。

## 这是正确的决定吗？

时间会证明我们是否做出了正确的决定。今天的 Go/No-Go 会议很热闹，并不是每个人都同意我们应该因此而推迟发布。就像我说的，我们没有什么信息可以继续。重要的是要注意，这个决定是作为一个团队做出的，而不是一个人的命令。Fedora 重视协作决策，这是一个很好的例子。

当周二公布细节时，我们可能会发现“哇，推迟发布不值得”。但我认为我们根据现有信息做出了最好的决定。

与此同时，请在 11 月 4 日至 5 日参加 [Fedora Linux 37 发布派对](https://fedoramagazine.org/youre-invited-to-the-fedora-linux-37-release-party/)。这会很有趣，即使版本还没有完全发布。