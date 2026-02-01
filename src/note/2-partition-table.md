# 安装系统

## 转移数据

在 Live 环境中，先将旧版用户文件夹重命名，然后启动安装程序。

### 磁盘分区

```
poplar@Greysia:~$ lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
zram0       251:0    0    8G  0 disk [SWAP]
nvme1n1     259:0    0  1.8T  0 disk 
├─nvme1n1p1 259:1    0   95M  0 part /boot/efi
└─nvme1n1p2 259:2    0  1.8T  0 part /home
                                     /boot
                                     /
nvme0n1     259:3    0  1.8T  0 disk 
└─nvme0n1p1 259:4    0  1.8T  0 part /bt
```

> [!NOTE]
> Fedora 使用 SwapOnZRAM，一般不需要手动创建 swap 分区/分区文件。

一般地，需要手动创建：

- ESP 分区：文件系统格式为 `EFI`，大小为 100MiB。

然后将剩余的空间新建一个 btrfs 文件系统（`MAIN_FS`），并新建三个子卷：

- ROOT：系统安装位置（`SYS_FS`）
    - 该子卷要格式化，格式化操作不会影响到其他的子卷。
- HOME：用于存储用户文件的子卷
    - 设置挂载点时不要勾选格式化选项
- boot：用于挂载 `/boot`

`/dev/nvme0n1p1` 挂载到 `/bt`

----

重装系统时，格式化并挂载 `/`、`/boot`、`/boot/efi`；仅挂载 `/home` 和 `/bt`。