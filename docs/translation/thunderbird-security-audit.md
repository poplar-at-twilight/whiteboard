---
comments: true
tags:
  - 新闻
  - Thunderbird
---

# K-9 Mail 与 OSTIF、7ASecurity 合作开展安全审计

- 译文信息：
    - 原文：[K-9 Mail Collaborates With OSTIF, 7ASecurity On Security Audit](https://blog.thunderbird.net/2023/07/k-9-mail-collaborates-with-ostif-and-7asecurity-security-audit/)
    - 作者：[Jason Evangelho](https://blog.thunderbird.net/author/jasonthunderbird-net/)
    - 许可证：[CC-BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/)
    - 译者：暮光的白杨
    - 日期：2023-07-21

---

![cover](./images/2023-07/K9-and-Thunderbird-Android-e1689868414831.jpg)

我们将 [K-9 Mail] 转变为 Thunderbird for Android 的过程不仅仅是改进用户界面和添加新功能。K-9 Mail 已经是 Android 开源生态系统的重要组成部分，而且由于它为 Thunderbird for Android 的未来奠定了基础，我们认为对该软件的安全健康进行投资非常重要。

为此，我们最近与开源技术改进基金（[OSTIF]）和 [7ASecurity] 合作，对 K-9 Mail 进行了广泛的安全审计。

[K-9 Mail]: https://k9mail.app/
[OSTIF]: https://ostif.org/
[7ASecurity]: https://7asecurity.com/

由 7ASecurity 六名审计员组成的团队努力工作，以识别并解决 K-9 Mail 中发现的任何潜在安全或稳定性问题。审计工作特别关注[威胁模型分析]、[模糊测试]（一种模拟现实世界中软件可能遇到意外或恶意输入的场景的技术）以及我们的软件供应链。

[威胁模型分析]: https://en.wikipedia.org/wiki/Threat_model
[模糊测试]: https://en.wikipedia.org/wiki/Fuzzing

我们很高兴地向大家报告，此次审计未发现任何高风险漏洞。安全审计确实发现了一些中低风险漏洞，其中大部分 K-9 Mail 团队已经解决或正在解决。

此外，我们很高兴与大家分享 OSTIF 得出的这一充满希望的结论：

>“Mozilla 有一个令人难以置信的基础来开始这个新篇章，因为报告指出了 7ASecurity 团队在参与过程中证明的 K-9 Mail 安全、健康实践和条件的七个广泛要点。”
>
><em>Amir Montazery, OSTIF</em>

整个过程既有教育意义又富有成效，我们衷心感谢与如此专业、知识渊博的团队合作。我们要向 OSTIF 的每一位员工致以最诚挚的谢意，包括 Amir Montazery、Ashley Leszkiewicz 和 Derek Zimmer。

我们衷心感谢 7ASecurity 的 Abraham Aranguren、Dariusz Jastrzębski、Daniel Ortiz、Miroslav Štampar 博士、Óscar Martínez 和 Patrick Ventuzelo 的辛勤工作和对细节的关注。

>“OSTIF 非常了解开源项目的运作方式，我们非常感谢他们能够参与进来，帮助我们协调 K-9 Mail 软件的安全审计工作。OSTIF 和 7ASecurity 是了不起的合作伙伴，他们提供了有益的指导，使审计过程变得轻而易举。我们非常感谢他们的专业精神和专业知识。我可以自信地说，我们计划再次与他们合作。”
>
><em>Ryan Sipes，Thunderbird 产品和业务开发经理。</em>

## 资源

- 在[此处]下载并阅读完整报告；
- [阅读 OSTIF 的博客文章]；
- [阅读 7ASecurity 的博客文章]；
- 了解有关 [Thunderbird for Android] 的更多信息。

[此处]: https://blog.thunderbird.net/files/2023/07/K9M-01-K-9-Mail-Mobile-Audit.pdf
[阅读 OSTIF 的博客文章]: https://ostif.org/k-9-mail-audit/
[阅读 7ASecurity 的博客文章]: https://7asecurity.com/blog/2023/07/mozilla-k-9-mail-audit/
[Thunderbird for Android]: https://blog.thunderbird.net/category/thunderbird-mobile/