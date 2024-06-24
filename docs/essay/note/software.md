---
comments: true
tags:
  - Linux
  - 笔记
---

# 软件包

## flatpak

安装：

```
sudo zypper in flatpak
```

添加远程仓库：

```
proxychains4 flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

加入 Flatpak 用户组

```
sudo usermod -aG flatpak $USER
```

查看用户手册：

```
flatpak --help
```

----

