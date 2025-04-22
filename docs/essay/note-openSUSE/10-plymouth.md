---
tags:
  - Linux
  - 笔记
---

# 删除 plymouth

卸载软件包：

```
sudo zypper rm -u plymouth
```
```
sudo rm -r /usr/lib/dracut/modules.d/50plymouth
```
```
sudo dracut -f
```

然后编辑 `/etc/default/grub` 文件：

删除 `GRUB_CMDLINE_LINUX_DEFAULT` 中的 `splash=silent` 和 `quiet` 参数。

然后运行：

```
sudo update-bootloader
```

```
sudo zypper al plymouth
```

----

参考文章：

- [How to disable Plymouth on Linux](https://linuxconfig.org/how-to-disable-plymouth-on-linux)
- [Update hung up on post trans update](https://forums.opensuse.org/t/update-hung-up-on-post-trans-update/180626)
- [Finally able to remove plymouth and successfully boot](https://forums.opensuse.org/t/finally-able-to-remove-plymouth-and-successfully-boot/167946)