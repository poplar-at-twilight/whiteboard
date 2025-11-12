# 检查系统信息

要列出 CPU 设备信息，可使用 `lscpu`。

要列出当前系统的块设备信息，可使用 `lsblk`，可附加 `--fs`。

要列出 USB 设备信息，可使用 `lsusb`。

要列出 PCI 设备信息，可使用 `lspci`。

要列出 RAM 信息，可使用 `free -m`。

## dmidecode

`dmidecode` 运行时需要 `root` 权限，可用于读取系统的硬件信息，例如内存条的频率。

使用 `sudo dmidecode -t` 查询可列出的硬件信息。

## hwinfo

`hwinfo` 与 `dmidecode`，使用 `sudo hwinfo --help` 列出可查询的信息。

