---
comments: true
tags:
  - 随笔
  - LibreOffice
  - Chrome
  - Fcitx5
  - vscode
  - Git
  - OpenJDK
  - Minecraft
  - Mindustry
  - 字体
  - 词典
  - sha256sum
---

# 记事本

## 关于标记语言与文档应用

个人看法：

- 简单，简短的文档使用 markdown + HTML 进行编写。
- 复杂，冗长或含有大量复杂样式的文档使用 LibreOffice 进行编写。

## Chrome

Google chrome 的用户数据文件在 `~/.config/chrome`，备份此文件夹即可。

## Fcitx5 词库

fcitx5 支持[搜狗输入法的细胞词库][词库]。可以在配置的词典设置中打开搜狗输入法的官网直接下载词库或者将转制的 `.dict` 文件放到 `~/.local/share/fcitx5/pinyin/dictionaries/` 中。然后重启 fcitx5。

[词库]: https://pinyin.sogou.com/dict/

## vscode

### 主题

群友发布了一个新的 vscode 主题（ConAntares.photonica），用起来感觉还不错。

### 白屏

如果 VSCode 启动后，主窗口为空白页。这个问题一般是因为 vscode 使用的 electron shell 与 GPU 硬件加速存在问题，使用下列命令启动 VSCode 即可：

```
$ code --disable-gpu
```

如果是在更新后发生这个问题，那么将 `~/.config/Code/GPUCache` 下的文件删除即可。

## git

突然发现 git 仓库的整体大小已经接近 1.3GB 了，是时候清理一下 git 历史记录了。

我使用了如下的命令：

```
git gc --aggressive
```

## Minecraft

### OpenJDK

最近在安装 minecraft 的时候，发现 Java8 的支持周期很长，以至于至今都没有停更。

- Java SE 8 (LTS) 于 2014 年发布，仅 Oracle 就会把它维护到 2030 年。

而且 openJDK 的[维护情况]也是有一点复杂，目前还在维护的就有 openJDK 8 (LTS)、11 (LTS)、17 (LTS)、19、20、21 (LTS) 六个版本。

[维护情况]: https://en.wikipedia.org/wiki/Java_version_history

同时根据 minecraft 维基[^m1]以及一些实机运行结果。理论上，

[^m1]: [Tutorials/Update Java](https://minecraft.fandom.com/wiki/Tutorials/Update_Java#Why_update?)

- minecraft 1.12 至 1.16.5 版本，需要 Java 8 (1.8.0) 或更新的版本；
- minecraft 1.17 至 1.17.1 版本，需要 Java 16 或更新的版本；
- minecraft 1.18 及更新的版本，需要 Java 17 或更新版本。

我所安装的是 minecraft 1.16.5 版本，选择的是 minecraft 维基推荐的 [Zulu OpenJDK] 中的 openJDK 15[^m2][^m3]。minecraft 的版本需要和 openJDK 互相匹配，同时高版本的 JDK 可以带来更好的性能。

[^m2]: Minecraft 1.16.5 在 openJDK 17 上无法运行。
[^m3]: 据说 minecraft 1.16.5 是在 Java 8 开发，但测试环境是 Java 11。

[Zulu OpenJDK]: https://www.azul.com/downloads/?package=jdk

预计 minecraft 1.16.5 和 1.12.2、1.7.10 一样，会成为一个诸多 mod 玩家汇聚的版本。

> TL;DR  
> ✅ Recommendation: Use [Adoptium Eclipse Temurin 17][jdk] and ensure that your local version matches the CI and production version.  
> Make sure, you have the latest patch level 17.0.3 or later, due to [CVE-2022-21449][cve].

[jdk]: https://adoptium.net/
[cve]: https://neilmadden.blog/2022/04/19/psychic-signatures-in-java/

### 启动器

这次我没选择 [HMCL] 作为游戏启动器，而是选择了 [PCL2]。就使用体验而言，还算不错。

[HMCL]: https://github.com/huanghongxun/HMCL
[PCL2]: https://github.com/Hex-Dragon/PCL2

![pcl2](./images/2023-02/PCL2.jpg)

### 光影包

由于在开启光影的情况下，如果再添加材质包会明显拉低游戏帧率（无法稳定 30FPS）[^m4]。所以我只能使用原版材质。帧率低于 40FPS，我会感受到明显的画面撕裂……

[^m4]: 当前设备的显卡是 MX150，算是一张已经入土的渣卡了。

光影包我选择的是 [BSL Shaders]。光影包不需要版本号严格地一一对应（它可以向下兼容旧版本），但材质包不行。BSL 的默认预设是 `High`，将它调成 Low[^m5] 就行了；同时在视频设置中将图像品质修改成流畅（默认是高画质）。

[^m5]: low 和 medium 画质预设其实差别很小，起码我感受不出来。但帧率却差了 5~10 FPS。

[BSL Shaders]: https://bitslablab.com/bslshaders/

=== "BSL 光影"

    ![01](./images/2023-02/2023-02-06_11.00.20.png)
    ![02](./images/2023-02/2023-02-06_11.00.54.png)

=== "OptFine 内置光影"

    ![03](./images/2023-02/2023-02-06_11.01.55.png)
    ![04](./images/2023-02/2023-02-06_11.02.20.png)

### 数据包与 mod

除了许多可用的 [Mod] 之外，1.16 也有许多的[数据包]（datapak）可用。

[Mod]: https://beta.curseforge.com/minecraft/search?index=1&gameId=432&pageSize=20&classId=6&sortType=2&gameVersion=1.16.5
[数据包]: https://minecraft.fandom.com/wiki/Data_pack

### Fabric

虽然本次安装 minecraft mod 时，我选择的是 [Forge] 和 [OptFine]。但查资料的时候发现 [fabric] 最好是和 [Iris Shaders] 与 [Sodium] [^m6] 一并使用；这可以说是来自开源社区的解决方案（实机运行的效果也和前者大差不差）。

[Forge]: https://files.minecraftforge.net/net/minecraftforge/forge/
[OptFine]: https://optifine.net/home
[fabric]: https://fabricmc.net/use/
[Iris Shaders]: https://irisshaders.net/index.html
[Sodium]: https://www.curseforge.com/minecraft/mc-mods/sodium

[^m6]: Sodium 是 Iris 的前置 mod。

### 外部链接

一些有用的外部链接：

- [MC 百科](https://www.mcmod.cn/)
- [MCBBS](https://www.mcbbs.net/)
- [Minecraft - Curseforge](https://www.curseforge.com/minecraft/modpacks)
- [Minecraft Wiki](https://minecraft.fandom.com/wiki/Minecraft_Wiki)
- [Minecraft Data Packs | Planet Minecraft Community](https://www.planetminecraft.com/data-packs/?p=0)

## Mindustry

[Mindustry] 是一个非常棒的 RTS 游戏[^jdk3]。但它不适合于在手机上玩，整个游戏更适合使用键鼠进行控制。

steam 上发布的版本是 DRM 加密的便携版。[官网][Mindustry]发布的是不完全的便携版[^m-2]或 jar 文件。由于我看不懂 json 文件，所以也没办法将 mindustry 配置成便携模式。

你可以[在 steam 购买 Mindustry][steam]！这是对于此游戏的认可与赞助💕，有益于开发者持续开发并改进游戏🤝！

[Mindustry]: https://mindustrygame.github.io/
[steam]: https://store.steampowered.com/app/1127400/Mindustry/

[^m-2]: 游戏本体与依赖的 Java runtime environment 是便携的，但是存档仍然保存在用户文档文件夹之中。
[^jdk3]: 它同样是基于 Java 开发的游戏。

### 修改 mindustry 的存档文件夹位置

要修改 mindustry 的存档位置，可以新建一个 CMD 文件，内容为：

```
set APPDATA=%CD%
mindustry.exe
```

其中，第一行表示将存档文件夹（`APPDATA`）路径设置为当前文件夹（`%CD%`）。第二行表示启动 mindustry.exe。

它也可以设置为：

```
set APPDATA=%CD%\save
java -jar mindustry.jar
```

- 参考：[How to change mindusty's save file location? - Skusci](https://www.reddit.com/r/Mindustry/comments/13v2udn/comment/jm71bpr/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button)

## 字体库

汉化组编写的高级字幕文件（ASS）经常使用多种字体，以下是一个实用的字体库：

- <https://cloud.404.website/>  
    * 超级字体整合包 XZ 下载地址：  
    ```
    magnet:?xt=urn:btih:bc72daa4f58fdda082c603ce6eee1d8199359434
    ```

## 词典

关于 goldendict 使用的词典文件，我一般是在 [download.freemdict.com] 获取免费分享的词典。

在阅读了一些帖子后，我认为，我至少需要以下词典：

- 英汉双解词典
- 汉英词典
- 网络词典

具体的方案就是：

- [简明英汉字典增强版]：用于快速查询单词释义；
- 朗道高阶英汉双解词典：用于交叉查询单词释义；
- 牛津高阶双解（第 9 版）：用于交叉查询单词释义；
- 韦氏高阶英汉双解词典：用于交叉查询单词释义；
- 现代汉英综合大辞典：用于查询中文词语对应的词汇；
- English Wikipedia：用于显示词汇的百科词条（尤其是无中文对应用语时）；
- Urban Dictionary：用于查询俚语和网络用语。

[download.freemdict.com]: https://downloads.freemdict.com/Recommend/
[简明英汉字典增强版]: http://github.com/skywind3000/ECDICT

## sha256sum

```
$ sha256sum <file>                  #计算文件的校验和
$ sha256sum --check <file.sha256>   #从文件中读取校验和并检验它们；或简写为 -c
$ sha256sum * > testing.sha256      #将校验和写入 sha256 文件
```