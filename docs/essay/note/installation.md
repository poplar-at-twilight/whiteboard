---
comments: true
tags:
  - Linux
  - 笔记
---

# 安装系统

如果出现 CJK 字符乱码，则语言选择 `en_US`。

- 不修改准备安装的软件包组合
- 新建帐户时，取消勾选自动登录
- 安装时，可以将 YaST2 的主题切换为深色

## 分区表

|分区名称|挂载点|分区大小|文件系统类型|
|---|---|---|---|
|root|`/`|虚拟机：16GiB 及以上<br />物理机：40GiB 或以上|推荐使用 `btrfs`|
|efi|`/boot/efi/`|不共用 ESP：100MiB<br />共用 ESP：256MiB|`fat32` 或 `vfat`|
|swap|`swap`|依照实际情况而定|`swap`|
|home|`/home`|依照实际情况而定|`xfs`、`ext4`、`btrfs` 等|

SWAP 分区推荐大小如下所示：

|系统物理内存（RAM）大小|推荐的 swap 分区大小|推荐的 swap 分区大小（如果需要休眠）|
|---|---|---|
|小于 2GB|RAM 的两倍|RAM 的三倍|
|2GB - 8GB|和 RAM 相同|RAM 的两倍|
|8GB - 64GB|RAM 的 0.5 倍|RAM 的 1.5 倍|
|大于 64GB|基于实际工作负载而定|不推荐休眠|

注意，第二块硬盘应当挂载到 `/home` 目录下。

## KVM

部署 KVM：

```
sudo zypper install -t pattern kvm_server kvm_tools
```
```
sudo usermod -aG libvirt $USER
```

然后重启系统。

注意，不要使用默认的存储池。另见：[openSUSE Wiki - KVM]

[openSUSE Wiki - KVM]: https://zh.opensuse.org/KVM