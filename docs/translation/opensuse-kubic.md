---
comments: true
tags:
  - 新闻
  - openSUSE
  - Kubic
---

# Kubic 项目已经结束

## 译文信息

- 源文：[Kubic Project Wound Down](https://kubic.opensuse.org/blog/2022-06-10-kubic-retired/)
- 作者：Richard Brown
- 许可证：No License
- 日期：2022-06-10
- 译者：暮光的白杨
- 翻译日期：2023-03-12

!!! note "关于翻译时间"

    关于此事我也是今日才得知，所以就把 Kubic 最后一篇新闻也收录至文档站。

---

<center><h2>公告</h2></center>

正如之前在 Kubic 项目邮件列表中讨论的那样，[Kubic 项目](https://kubic.opensuse.org)现已正式结束。

我们不再提供下载 Kubic，也将不再维护 Kubic。

[openSUSE MicroOS](https://microos.opensuse.org/) 曾经是 Kubic 项目的一个分支，现在对于我们这些以前需要在它们之间分担注意力的贡献者来说，将发挥更突出的作用。

我们建议希望在 openSUSE 基础之上运行 [kubernetes](https://kubernetes.io/) 工作负载的用户安装 [openSUSE MicroOS](https://microos.opensuse.org/)，然后安装 [k3s](https://rancher.com/docs/k3s/latest/en/installation/install-options/#options-for-installation-with-script)。

喜欢 Kubic 项目以前提供的 kubernetes RPM 包和容器的用户可以继续使用它们，因为它们将由新的社区维护者继续维护。请理解这项努力完全是自愿的，并且是“尽最大努力”。我们预计这不会像 Kubic 以前提供的那样完美。有关使用这些软件包的任何安装、迁移或升级步骤的确切细节将通过 Kubic 邮件列表和 [openSUSE wiki](https://en.opensuse.org/Portal:Kubic) 进行交流。

由 MicroOS 和 Kubic 项目团队共同维护的所有 “non-kubernetes” openSUSE 官方容器将继续为 MicroOS 维护和支持。您可以一如既往地直接从 [registry.opensuse.org](https://registry.opensuse.org/cgi-bin/cooverview?srch_term=project%3D%5EopenSUSE%3AContainers%3A+container%3Dopensuse%5C%2F%2F*) 获取它们。

感谢大家多年来的贡献，我们期待着看到我们可以用这种更有针对性的方法将 MicroOS 带到何处。

**Kubic/MicroOS 团队**