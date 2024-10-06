---
tags:
  - Linux
  - 笔记
---

# 在 btrfs 文件系统上使用 swapfile

根据 [Swapfile - BTRFS documentation]，先在 `/home` 下新建一个 `swap` 文件夹。

[Swapfile - BTRFS documentation]: https://btrfs.readthedocs.io/en/latest/Swapfile.html

创建 `swapfile`：

```shell
sudo btrfs filesystem mkswapfile --size 4G /home/swap/swapfile
```

然后启用 `swapfile`：

```shell
sudo swapon /home/swap/swapfile
```

查看正在使用的 `swapfile`：

```shell
sudo swapon -s
```

关闭特定的 `swapfile`：

```shell
sudo swapoff /home/swap/swapfile
```