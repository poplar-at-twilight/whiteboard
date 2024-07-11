---
comments: true
tags:
  - 杂谈
  - Minecraft
  - openSUSE
---

# 在 openSUSE 上游玩 Minecraft

## 准备

使用的启动器是 [MultiMC] 发布的 `Binary Tarball (64-bit)` 版本，将文件解压至 `~/bin/mc`。

[MultiMC]: https://multimc.org/

### JDK

openSUSE 默认安装的 LibreOffice 会依赖 OpenJDK 21。一般不需要安装额外的 JDK。

### desktop 文件

使用 `DRI_PRIME=1` 变量让 Minecraft 在 GPU 下运行。

```
[Desktop Entry]
Comment[zh_CN]=Minecraft launcher
Categories=Game
Comment=Minecraft launcher
Exec=env DRI_PRIME=1 /home/poplar/bin/mc/MultiMC
GenericName[zh_CN]=MultiMC
GenericName=MultiMC
Icon=minecraft
MimeType=
Name[zh_CN]=MultiMC
Name=MultiMC
Path=/home/poplar/bin/mc
StartupNotify=true
Terminal=false
TerminalOptions=
Type=Application
X-KDE-SubstituteUID=false
X-KDE-Username=
```

## 启动器配置

- 语言：简体中文
- JDK：
    - Java 路径：使用系统的 JDK
    - 最大内存分配大小：4096MiB
- Minecraft：将窗口大小调整为 1920x1080
- 代理服务器：
    - HTTP
    - 127.0.0.1:7890
- 账户：Microsoft 账户

## 安装游戏

MultiMC 以实例（instance）为单位管理 Minecraft 游戏本体。默认安装的版本是 1.20.6。

游戏本体存放在 `~/bin/mc/instances/<版本名称>/.minecraft` 中。

### mod

- Fabric
- [Fabric API](https://modrinth.com/mod/fabric-api)
- [Iris Shader](https://modrinth.com/mod/iris)
- [Sodium](https://modrinth.com/mod/sodium)
- [Lithium](https://modrinth.com/mod/lithium)
- [Journeymap](https://www.curseforge.com/minecraft/mc-mods/journeymap)

### 光影包

默认选择：[Complementary Shaders - Reimagined]

[Complementary Shaders - Reimagined]: https://modrinth.com/shader/complementary-reimagined

## 外部链接

一些有用的外部链接：

- [MC 百科](https://www.mcmod.cn/)
- [MCBBS](https://www.mcbbs.net/)
- [Minecraft - Curseforge](https://www.curseforge.com/minecraft/modpacks)
- [Minecraft Wiki](https://minecraft.fandom.com/wiki/Minecraft_Wiki)
- [Minecraft Data Packs | Planet Minecraft Community](https://www.planetminecraft.com/data-packs/?p=0)