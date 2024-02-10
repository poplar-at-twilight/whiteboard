---
comments: true
tags:
  - 杂谈
  - RSS
---

# 基于 RSS 的简单资讯聚合订阅

在 2022 年使用 RSS 订阅资讯对于大部分人来说已经不是一个合适选择了。但 RSS 作为一种古老的订阅方式仍有其价值。比如订阅博客和其他一些更新动态。

## 为什么要用 RSS

因为没有广告啊，而且订阅内容可以自己控制。🤣

## RSS 客户端

[Fluent Reader](https://github.com/yang991178/fluent-reader) 是一个具有现代化 UI 的 RSS 订阅器，支持 Windows、Linux 和 MacOS，支持自动浅/深色模式切换和在线订阅服务。

## 寻找 RSS 订阅地址

有了 RSS 阅读器后，下一步就是去寻找 RSS 订阅链接。

对于 GitHub 而言，如果需要关注特定的某个项目的发行动态，可以在项目的发布页地址后面加上 `atom`，比如：`https://github.com/yang991178/fluent-reader/releases.atom`。对于一般性的网站，则可以直接在网站地址的末尾加上 rss 或 feed 来测试该网站是否支持 RSS 订阅；或在网站内搜索是否有 `rss`、`feed` 或 `RSS 订阅`相关的跳转链接（它们一般被放在网站的顶部或者底部）。

### 其他方法

1. [RSSHub-Radar](https://github.com/DIYgod/RSSHub-Radar) 是一个可以快速搜索当前站点是否存在 RSS 订阅的浏览器扩展。
2. [RSSHub](https://docs.rsshub.app/) 是一个开源的 RSS 订阅生成器。  
    它可以为大量本来不支持 RSS 订阅的站点（比如 B 站、知乎等）生成 RSS 订阅。RSSHub 官方自己有维护一个 [RSSHub 实例](https://rsshub.app/) 和一个丰富的[订阅路由表](https://docs.rsshub.app/social-media.html)（每日都在更新）；你可以直接使用官方的服务器或者自建服务器。

## 其他

### 内容显示不全

要完整显示 RSS 订阅的内容，你有时需要订阅器加上一层代理。比如：

>"E:\Software\Toolkit\Fluent Reader\Fluent Reader.exe" --proxy-server=http://127.0.0.1:7890

或者在 Fluent Reader 的 *设置>订阅源* 中，修改**订阅文章打开方式**。

### 更新间隔

[一座桥在水上](https://github.com/yzqzss)在他的一篇[博客](https://blog.othing.xyz/archives/rss-refresh.html)中详细说明了为什么要提高 RSS 更新间隔的原因，这里就不赘述了。

对于 RSS 订阅者，我们倡议：

>延长 rss 自动更新时间，最好1小时以上---推荐设为1.5小时。  
>仅对即时性要求较高的资讯源或论坛源适当缩短刷新间隔。  
>让更多的 rss 同好们一起提高刷新间隔。  
>让 rss 软件开发者也知道此事。

### 一些关联的项目

- [rss-sub-list](https://github.com/saveweb/rss-list)  
    独立博客&播客全订阅计划！
- [声冻计划](https://blog.othing.xyz/archives/freeze_wave_project_start.html)