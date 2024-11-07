---
tags:
  - Linux
  - 笔记
---

# 初次启动

## 切换默认会话

**点击左下角的按钮，将 KDE 默认会话从 X11 修改为 wayland。**

## 手动创建用户

如果出现需要手动创建用户时，若要使用户名与用户所属默认组组名相同，需要先创建新的用户组。

## 设置主机名

```
sudo hostnamectl set-hostname --pretty "White-Poplar's Laptop"
```
```
sudo hostnamectl set-hostname --static c004-h1
```

## 删除 Packagekit 和 Discover

```
sudo zypper rm -u PackageKit discover6; sudo zypper al discover6 PackageKit
```

## 删除软件源，更新系统

删除多余的 `repo-openh264` 软件源和 ISO 软件源。

更新系统：

```
sudo zypper ref; sudo zypper dup
```

补齐缺失的软件包：

```
sudo zypper inr
```

## 多媒体播放器

添加 Packman 源（详见 [bashrc]）：

[bashrc]: ./8-shell-script.md

```
add-packman
```

更新并安装多媒体播放器：

```
sudo zypper refresh
```
```
update-packman
```
```
sudo zypper in mpv ffmpeg-7
```

## 音量控制故障

如果播放器音量大小与系统设置的音量值不呈线性变化，则使用 `alsamixer` 将 `Base Speaker` 的值降到 0。

## 更换语言（可选操作）

在 YaST Language 将首选语言修改为简体中文，同时选择英文作为第二语言。

然后将 KDE 设置中的区域和语言更改为中文。

等待更新完成，重启系统。

## AMD GPU

以核显运行应用的环境变量：

```
DRI_PRIME=0
```

以独显运行程序的环境变量：

```
DRI_PRIME=1
```