---
comments: true
tags:
  - 杂谈
  - Sony Xperia
  - 手机
---

# Sony Xperia 实用工具

|名称|用途|
|---|---|
|[Newflasher](https://forum.xda-developers.com/t/tool-newflasher-xperia-command-line-flasher.3619426/)|系统固件命令行强刷工具|
|[Xperifirm](https://xperifirmtool.com/)|系统固件下载工具（需要代理）|
|[payload-dumper-go](https://github.com/vm03/update_payload_extractor)|Payload 一键解压工具|
|[Magisk 安装向导](https://topjohnwu.github.io/Magisk/install.html)|root 系统工具|
|[Sony xperia driver](https://developer.sony.com/develop/drivers/)|Sony Xperia 设备驱动</br>[ADB&Fastboot driver for sony xz2](https://developer.sony.com/file/download/xperia-xz2-driver/)|

- [Sony Open Devices Project](https://developer.sony.com/)

----

针对 Sony XZ2：

- [Sony Xperia XZ2 ROMs, Kernels, Recoveries, & Other](https://forum.xda-developers.com/f/sony-xperia-xz2-roms-kernels-recoveries-other.7464/)
- [LineageOS for Sony XZ2 (akari)](https://wiki.lineageos.org/devices/akari/)
- 蓝灯模式/Bootloader/Fastboot/Download：关机，按住 `音量加键`，用 USB 数据线连接至电脑；
- 绿灯模式/RECOVERY/Flash mode：关机，同时按住 `音量减键` 和 `电源键`；  
    - 关机后，按住音量键和电源键，等到 SONY 标志出现时立即松开，即可进入 RECOVERY 模式；  
- 强刷完固件后，如果要刷 ROOT，此时不要开机，继续安装 TWRP；  
- 如果卡在 SONY logo 界面，没有任何反应，可以同时按住`音量减键`、`音量加键`和`电源键`，强制关机；
- 设备驱动使用 Sony 官方和 newflasher 自带的驱动即可。