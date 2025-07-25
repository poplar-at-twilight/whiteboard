---
tags:
  - Linux
  - 笔记
---

# KDE 相关

## 杂项

### TTY

按下 Ctrl + Alt + F1 ~ F9 在不同的 TTY 中切换。

### 启动动画

要关闭应用的启动动画，可以在 desktop 文件中新增：`StartupNotify=false`，或在 **编辑应用程序** > **应用程序** > **高级选项**，关闭 **启用启动特效**。

### 图标主题

- [Qogir-cursors-theme]
- [ ] [Tela-icon-theme]
- [x] `papirus-icon-theme`
    - 解压文件保存位置：`~/.local/share/icons`

[Tela-icon-theme]: https://store.kde.org/p/1279924
[Qogir-cursors-theme]: https://www.pling.com/p/1366182/

### 某些快捷键

- 杀死窗口：**Super** + **Ctrl** + **Esc**
- 切换窗口：**Alt** + **Tab**

### 以太网

新建有线以太网连接时，需要在 **仅限使用此设备** 中指定设备（`enp5s0`）。

----

## KDE 设置

鼠标和触摸板 > 触摸板：

- 反向滚动（自然滚动）

键盘 > 键盘：

- NumLock 在 Plasma 启用时的状态：打开

键盘 > 虚拟键盘：

- Fcitx 5

声音：

- 静音麦克风

显示和监视器：

- 缩放率：150%

颜色和主题，修改：

- 图标
- 光标
- 欢迎屏幕
- 登录屏幕

壁纸：

- 幻灯片

常规行为：

- 单击文件或者文件夹时：打开

锁屏：

- 自动锁定屏幕：15 分钟
- 外观

用户反馈：

- 第四档

日期和时间：

- 自动设置日期和时间

电源管理：

- 交流供电：
  - 不活动一段时间后：无操作
  - 按下电源键时：关闭屏幕
  - 合上笔记本盖时：锁屏

用户：

- 更换头像

自动启动：

- qBittorrent
- telegram
- FlClash

会话 > 桌面会话：

- 位置：将默认位置重定向到当前文件夹
- 登录时自动启动应用程序：启动为空会话