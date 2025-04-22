---
tags:
  - Linux
  - 笔记
---

# 安装系统

## 转移数据

在 Live 环境中，先将旧版用户文件夹重命名，然后启动安装程序。

### 磁盘分区

```
poplar@Greysia:~$ lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
zram0       251:0    0    8G  0 disk [SWAP]
nvme1n1     259:0    0  1.8T  0 disk 
├─nvme1n1p1 259:1    0  100M  0 part /boot/efi
├─nvme1n1p2 259:2    0    1G  0 part /boot
├─nvme1n1p3 259:3    0  1.8T  0 part /home
└─nvme1n1p4 259:4    0   39G  0 part /
nvme0n1     259:5    0  1.8T  0 disk 
└─nvme0n1p1 259:6    0  1.8T  0 part /home/bt
```

一般地，需要手动创建：

- ESP 分区：文件系统格式为 EFI，大小为 100MiB。
- BOOT 分区：文件系统格式为 ext4，大小为 1GiB。
- SYS 分区：类型设置为 btrfs 卷，大于 20GiB 即可。

挂载：

- /home
- /home/bt

不需要手动创建的内容：

- SWAP