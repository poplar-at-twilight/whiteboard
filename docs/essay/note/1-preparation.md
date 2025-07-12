---
tags:
  - Linux
  - 笔记
---

# 准备

## ISO 文件

因为 Fedora 不会更新 ISO 文件，所以不必频繁下载新版 ISO 文件。

下载：

- <https://mirror.nyist.edu.cn/fedora/releases/42/KDE/x86_64/iso/Fedora-KDE-42-1.1-x86_64-CHECKSUM>
- <https://mirror.nyist.edu.cn/fedora/releases/42/KDE/x86_64/iso/Fedora-KDE-Desktop-Live-42-1.1.x86_64.iso>

## 校验文件

```
wget https://fedoraproject.org/fedora.gpg
```

```
gpgv --keyring ./fedora.gpg Fedora-KDE-42-1.1-x86_64-CHECKSUM
```

```
sha256sum --ignore-missing -c Fedora-KDE-42-1.1-x86_64-CHECKSUM
```

## 烧录 ISO 文件

使用 [ventoy] 创建可启动安装介质，启动模式为正常启动。

[ventoy]: https://github.com/ventoy/Ventoy

## 切换引导启动项

关机后，按下开机键，同时按 `Esc` 进入 BIOS 界面，切换启动项。

### 备份文件

使用 LiveCD 环境将旧用户文件夹 `poplar` 重命名 `poplar.old`。