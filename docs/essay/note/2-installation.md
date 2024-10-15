---
tags:
  - Linux
  - 笔记
---

# 安装系统

如果出现 CJK 字符乱码，则语言选择 `en_US`。

或者按 **E** 编辑 grub 条目，在 linux 所在行的末尾添加 `lang=zh_CN`，再按 **Ctrl** + **X** 重新引导启动 YaST2。

注意：

- 不修改准备安装的软件包组合
- 新建帐户时，取消勾选自动登录
- 安装时，可以将 YaST2 的主题切换为深色

## 分区表

|分区名称|挂载点|分区大小|文件系统类型|
|---|---|---|---|
|root|`/`|虚拟机：16GiB 及以上<br />物理机：40GiB 或以上|`btrfs`|
|efi|`/boot/efi/`|不共用 ESP：100MiB<br />共用 ESP：256MiB|`fat32` 或 `vfat`|
|swap|`swap`|参考下文|`swap`|
|swapfile|`/home/swap`|详见 [在 btrfs 文件系统上使用 swapfile](./9-swapfile-on-btrfs.md)|同左|
|home|`/home`|依照实际情况而定|`xfs`（对于 HDD 而言）、`btrfs`（对于 SSD 而言）|

swapfile/SWAP 分区参考大小如下所示[^ref_rhel]：

[^ref_rhel]: 参考 [14.2. Recommended system swap space - redhat documentation]

[14.2. Recommended system swap space - redhat documentation]: https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/managing_storage_devices/getting-started-with-swap_managing-storage-devices#recommended-system-swap-space_getting-started-with-swap


|系统物理内存（RAM）大小|推荐的 swap 分区大小|推荐的 swap 分区大小（如果需要休眠）|
|---|---|---|
|⩽ 2 GiB|RAM 的两倍|RAM 的三倍|
|> 2 GiB – 8 GiB|和 RAM 相同|RAM 的两倍|
|> 8 GiB – 64 GiB|至少 4GiB|RAM 的 1.5 倍|
|> 64GiB|至少 4GiB|不推荐休眠|

注意，第二块硬盘应当挂载到 `/home` 目录下。

## 样例

```
poplar@c004-h1:~> lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
nvme1n1     259:0    0 953.9G  0 disk 
├─nvme1n1p1 259:1    0   256M  0 part /boot/efi
├─nvme1n1p2 259:2    0    40G  0 part /var
│                                     /usr/local
│                                     /root
│                                     /srv
│                                     /opt
│                                     /boot/grub2/x86_64-efi
│                                     /boot/grub2/i386-pc
│                                     /.snapshots
│                                     /
└─nvme1n1p3 259:3    0 913.6G  0 part /home
nvme0n1     259:4    0   1.8T  0 disk 
└─nvme0n1p1 259:5    0   1.8T  0 part /home/bt
poplar@c004-h1:~> sudo swapon -s
Filename                                Type            Size            Used            Priority
/home/swap/swapfile                     file            4194300         873340          -2
```