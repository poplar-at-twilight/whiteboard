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

烧录工具选择 [ventoy]。

[ventoy]: https://github.com/ventoy/Ventoy