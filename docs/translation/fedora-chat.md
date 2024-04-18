---
comments: true
tags:
  - 新闻
  - Fedora
---

# Fedora Chat：通往 Matrix 的门户

- 译文信息：
    - 源文：[Fedora Chat: Your Gateway to Matrix](https://fedoramagazine.org/fedora-chat-your-gateway-to-matrix/)
    - 作者：[Robert Wright](https://fedoramagazine.org/author/rwright/) 和 [David Cantrell](https://fedoramagazine.org/author/dcantrell/)
    - 许可证：[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
    - 译者：暮光的白杨
    - 日期：2024-04-18

----

> ![](./images/2024-04/gateway.jpg)
> 照片由 [Tom Gainor] 在 [Unsplash] 上发布

[Tom Gainor]: https://unsplash.com/@its_tgain?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash
[Unsplash]: https://unsplash.com/photos/arch-landmark-N9PCtj8wdFg?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash

## Matrix 是什么？

[Matrix] 是基于互操作性和去中心化原则的开放式去中心化安全通信协议。您可以在一个主服务器（home server, homeserver）上创建一个账户，然后加入不同主服务器的频道。这意味着如果您在 Matrix.org 上有一个账户，则可以使用它加入我们的社区空间！Fedora Chat 就是其中之一。

[Matrix]: https://matrix.org/

[Matrix 基金会]是 Matrix 协议的守护者，确保其始终是安全、去中心化通信的自由和开放标准。基金会负责开发和维护 Matrix 规范，并与社区密切合作，以增强互操作性和创新性。

[Matrix 基金会]: https://matrix.org/about/

## Fedora Chat 是什么？

Fedora Chat 是我们的 Matrix 主服务器实例。它由两个主服务器组成，一个是我们的 fedoraproject.org 社区空间，另一个是 fedora.im 主服务器，为我们的用户提供加入 fedoraproject.org 空间的账户。这两个服务器都是由 [Element Matrix Services][ems] ("EMS") 托管的，这要感谢红帽公司对我们的赞助。

[ems]: https://element.io/

您可以使用 fedora.im 账户加入其他 Matrix 主服务器，并与其他社区（包括但不限于 [Gnome]、[Mozilla] 和 [Ubuntu] 等）协作！

[Gnome]: https://wiki.gnome.org/GettingInTouch/Matrix
[Mozilla]: https://wiki.mozilla.org/Matrix
[Ubuntu]: https://ubuntu.com/community/communications/matrix

## 开始使用 Fedora Chat

准备好加入 Fedora Chat 了吗？如果您还没有 Matrix 账户，您可以使用 fedora.im 主服务器获得一个账户并加入我们的社区空间！如果您已经拥有 Matrix——[您可以通过此链接加入我们的 Fedora 空间][exlink]。

[exlink]: https://matrix.to/#/#fedora-space:fedoraproject.org

1. 访问 <https://chat.fedoraproject.org>，点击蓝色的 "Sign in"，然后点击 "Continue with your Fedora Account"，使用您的 FAS 账户登录。
1. 您应该会自动加入我们的空间，这是一个聊天室集合，如果没有，请在这里加入 [#fedora-space:fedoraproject.org]。
1. 然后，您可以在菜单中选择不同的聊天室。找到一个聊天室，然后打招呼吧！

[#fedora-space:fedoraproject.org]: https://matrix.to/#/#fedora-space:fedoraproject.org

## 探索其他 Matrix 客户端

虽然 Fedora Chat 使用的是托管的 Element 客户端，但 Matrix 的世界是广阔的。探索适合您需求的，从桌面应用到移动应用的其他客户端解决方案。每个客户端都提供独特的功能，让您可以定制自己的体验。您可以[在 Matrix.org 网站上找到不同客户端的最新列表][client]。

[client]: https://matrix.org/ecosystem/clients/

## 了解加密、密钥和设备验证

Matrix 的端到端加密可确保只有聊天室内的通信用户才能读取信息。有些聊天室是端到端加密的，但大多数公共聊天室不是。Fedora 社区空间的大多数聊天室都不是端对端加密的，您可以看到历史记录。但是，如果您直接给其他用户发送消息或加入一个端对端加密聊天室，您的消息默认是加密的。这是通过管理加密密钥实现的，加密密钥可确保每次对话的安全。加密过程包括生成由客户端代为存储的唯一密钥。

保护您的加密密钥至关重要，而您的恢复密钥也是至关重要的。如果您丢失了主密钥备份，恢复密钥可以让您恢复对加密对话的访问。请将恢复密钥保存在安全的地方。

会话验证允许用户在多个设备上验证自己的身份，确保经过验证的设备只能读取加密信息，从而进一步增强了安全性。

[Element 提供了关于如何创建和管理密钥备份的常见问题解答][faq]，但这会根据您选择使用的客户端而有所不同。如果您使用 chat.fedoraproject.org，这是一个很好的资源。

[faq]: https://element.io/help#encryption

## 下一步行动

加入 [#fedora-space:fedoraproject.org] 并在 [#intros:fedoraproject.org] 打招呼——欢迎来到 Matrix 世界！

[#intros:fedoraproject.org]: https://matrix.to/#/#intros:fedoraproject.org