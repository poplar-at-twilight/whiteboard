---
tags:
  - Linux
  - 笔记
---

# 准备工作

## 下载、校验 ISO 文件

```
iso-m
```

### openSUSE 项目的公钥

```
wget https://download.opensuse.org/tumbleweed/repo/oss/gpg-pubkey-29b700a4-62b07e22.asc
```

## 烧录 ISO 文件

使用 [ventoy] 创建可启动安装介质，启动模式为正常启动。

[ventoy]: https://github.com/ventoy/Ventoy

## 切换引导启动项

关机后，按下开机键，同时按 `Esc` 进入 BIOS 界面，切换启动项。

### 备份文件

使用 LiveCD 环境将旧用户文件夹 `poplar` 重命名 `poplar.old`。