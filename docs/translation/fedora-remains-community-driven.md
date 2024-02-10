---
comments: true
tags:
  - 新闻
  - Fedora
  - TheEvilSkeleton
  - 社区动态
---

# Fedora 项目仍然是社区驱动的

## 作品信息

- 原文：[The Fedora Project Remains Community Driven](https://theevilskeleton.gitlab.io/2022/09/30/the-fedora-project-remains-community-driven.html)
- 作者：[TheEvilSkeleton](https://theevilskeleton.gitlab.io/about)
- 许可证：[CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/legalcode)
- 译者：暮光的白杨
- 日期：2022-10-18

----

## 简介

最近，Fedora 项目从他们的 Mesa 构建中删除了所有获得专利的编解码器，而没有征求其他社区成员的意见。这一决定遭到社区的严厉批评。对于这个决定，一些人甚至[要求](https://www.reddit.com/r/Fedora/comments/xqqp7c/please_remove_the_community_driven_part_from/) Fedora 项目从其官方描述中删除“社区驱动”。我想花一些时间来解释为什么在我看来这个决定是完全合理的，以及 Fedora 项目如何保持社区驱动。

## 法律合规性不能投票

像 Fedora 项目这样的大型组织必须遵守法律，以尽可能避免诉讼。[专利流氓](https://en.wikipedia.org/wiki/Patent_troll)真的很普遍，并且会针对大型组织下手。我们不要忘记，在 2019 年，GNOME 被[专利流氓](https://en.wikipedia.org/wiki/GNOME_Foundation#GNOME_Patent_Troll_Defense_Fund)起诉。不幸的是，专利流氓很普遍。更糟糕的是，自今年年初以来，针对开源项目的专利流氓行为[大幅增加](https://www.zdnet.com/article/patent-troll-attacks-against-open-source-projects-are-up-100-since-last-year-heres-why/)。因此，这一决定必须迅速采取行动，以尽快避免潜在的诉讼。

遵守法律不是由社区来决定的。例如，Arch Linux，另一个社区驱动的发行版，不能也不会重新分发专有软件，除非他们得到软件作者的许可。这不是可以投票的东西，而是必须遵守的。这并不意味着 Arch Linux 不是社区驱动的。它只意味着它受法律约束，就像 Fedora 项目无法发布这些专利编解码器一样。

!!! quote "关于 ArchLinux"
    此处以 Debian 为例会更加合适一些，ArchLinux 给我的印象就是不在乎这边琐碎的细节（包括但不限于重新分发 nVidia 的固件和各种编解码器），他们也没有可供诉讼的实体组织，只关注实用性和简洁。  
    —— 译者，2022-10-18.

即使 Fedora 项目在过去几年没有被起诉，但这并不意味着他们将来会继续免于诉讼。 专利流氓的增加是 Fedora 项目迅速对此做出反应的一个很好的理由。如果他们被起诉，社区会为聘请律师的费用买单吗？

## 社区驱动

作为一名与 Red Hat 无关的 Fedora 项目志愿者，我相信 Fedora 项目仍然是社区驱动的。我目前是 [Fedora 网站和应用程序团队](https://docs.fedoraproject.org/en-US/websites/)的一员，在即将到来的[网站改造存储库](https://gitlab.com/fedora/websites-apps/fedora-websites/fedora-websites-3.0)中担任[“开发者”角色](https://gitlab.com/fedora/websites-apps/fedora-websites/fedora-websites-3.0/-/project_members?search=TheEvilSkeleton)。这主要是一项志愿者工作，因为我们中的大多数贡献者都与红帽没
有关系，是没有报酬的开发者。

由于我们（志愿者）是控制决策的人，我们可以故意让网站看起来令人不快和骇人听闻。当然，我们关心 Fedora 项目，因此我们希望它对潜在用户、贡献者甚至愿意转向开源的企业具有吸引力。

最近，我[提出](https://github.com/fedora-silverblue/silverblue-site/issues/114#issuecomment-1246879559)将 [Silverblue](https://silverblue.fedoraproject.org/)、[Kinoite](https://kinoite.fedoraproject.org/)、[CoreOS](https://getfedora.org/en/coreos) 等页面的布局统一为一个在导航时看起来统一且一致的布局，例如相同的导航栏、页脚、调色板等。一些开发人员正在考虑加入这项工作，但有些人不同意。当然，这只是一个提议，但如果每个人都参与进来，那么我们志愿者将成为领导这一倡议的人。

这是我个人经历中的一个例子，但许多倡议都是（并且将会）由独立贡献者提出的，并且也可以领导这项工作。非法律提案仍在民主投票中，调查仍然受到重视。目前，Fedora 项目正在从 Pagure 迁移到 GitLab，并从 IRC 迁移到 Matrix。那是因为社区对此进行了投票。我表达了我的观点，并且是在调查中提出这两项更改的人之一。

## 结论

我完全同意 Fedora 项目关于禁用 Mesa 专利编解码器的决定。社区不能也不应该要求这些更改，因为这是关于潜在诉讼的法律讨论。只要您遵守美国法律（不幸的是）和 Fedora 行为准则，任何合法的东西都会由社区民主投票。