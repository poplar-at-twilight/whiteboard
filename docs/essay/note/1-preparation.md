---
comments: true
tags:
  - Linux
  - 笔记
---

# 准备工作

## 下载 ISO 文件

下载地址：

- <https://mirror.sjtu.edu.cn/opensuse/tumbleweed/iso/>

Live 环境可以使用 [Fedora KDE spin] 或者 openSUSE Tumbleweed KDE Live DVD。

[Fedora KDE spin]: https://spins.fedoraproject.org/kde/

## 校验文件

### 下载证书

下载 openSUSE 项目的公钥：

```
wget https://download.opensuse.org/tumbleweed/repo/oss/gpg-pubkey-29b700a4-62b07e22.asc
```

该公钥的指纹应当为：

```
pub   rsa4096 2022-06-20 [SC] [有效至：2026-06-19]
      AD48 5664 E901 B867 051A  B15F 35A2 F86E 29B7 00A4
uid             [ 完全 ] openSUSE Project Signing Key <opensuse@opensuse.org>
```

### 校验 ISO

使用下列命令计算校验和：

```
check-iso
```

* shell 脚本源码另见[此处](./8-shell-script.md)。

### 校验签名

安装工具：

```
sudo zypper in gpg2
```

导入证书：

```
gpg --import gpg-pubkey-29b700a4-62b07e22.asc
```

检查证书：

```
gpg -k
```

校验签名：

```
gpg --verify openSUSE-Tumbleweed-DVD-x86_64-[快照序号]-Media.iso.sha256.asc
```

## 烧录 ISO 文件

使用 [ventoy] 创建可启动安装介质，启动模式为正常启动。

[ventoy]: https://github.com/ventoy/Ventoy

## 额外的 RPM 包

- <https://github.com/ilya-zlobintsev/LACT/releases>
- <https://github.com/Clash-Verge-rev/clash-verge-rev/releases>

## 切换引导启动项

关机后，按下开机键，同时按 `Esc` 进入 BIOS 界面，切换启动项。

### 备份文件

使用 LiveCD 环境将旧用户文件夹 `poplar` 重命名 `poplar.old`。