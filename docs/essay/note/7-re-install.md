---
tags:
  - Linux
  - 笔记
---

# 重装

```
poplar@c004-h1:~> id
uid=1000(poplar) gid=1000(poplar) 组=1000(poplar),108(libvirt),476(flatpak)
```

## 文件备份清单

注意，GPG 密钥链需要手动重新导入。

```
/home/poplar/bin
/home/poplar/Desktop
/home/poplar/Documents
/home/poplar/Downloads
/home/poplar/Misc
/home/poplar/Music
/home/poplar/Pictures

/home/poplar/.config/audacious
/home/poplar/.config/chromium
/home/poplar/.config/crow-translate
/home/poplar/.config/fcitx
/home/poplar/.config/fcitx5
/home/poplar/.config/goldendict
/home/poplar/.config/keepassxc
/home/poplar/.config/MangoHud
/home/poplar/.config/pip
/home/poplar/.config/qBittorrent

/home/poplar/.local/share/applications
/home/poplar/.local/share/fcitx5
/home/poplar/.local/share/flatpak
/home/poplar/.local/share/fonts
/home/poplar/.local/share/goldendict
/home/poplar/.local/share/icons
/home/poplar/.local/share/konsole
/home/poplar/.local/share/qBittorrent
/home/poplar/.local/share/Steam

/home/poplar/.steam
/home/poplar/.var

/home/poplar/.bashrc
/home/poplar/.gitconfig
/home/poplar/.steampath
/home/poplar/.steampid

/home/bt
/home/swap
```