---
tags:
  - Linux 简明指南
  - 安装系统
  - 教程
---

# 准备工作

本文主要描述安装 Linux 前应当做的一些预备工作。

## 最低硬件需求

对于本项目所涉及的 Linux 发行版的最低硬件要求如下所示：

openSUSE Tumbleweed:

- 1.6GHz 或更高的处理器（推荐奔腾 4 2.4GHz 或更高的处理器或任何 AMD64 或 Intel64 处理器）；
- 1GB 物理内存（推荐 4GB 或以上）；
- 10GB 的可用磁盘空间用于最小的安装，16GB 的可用磁盘空间用于图形化桌面（日常使用建议 40GB 或更多）；
- 支持大多数现代声卡和显卡，800x600 的显示分辨率（推荐 1024 x 768 或更高）。

Fedora Workstation:

- 2GHz 双核处理器或更快的处理器（推荐使用 2GHz 四核或更快的处理器）；
- 2GB 物理内存（推荐 4GB 或以上）；
- 15GB 磁盘空间（推荐 20GB 或以上）；
- 支持大多数现代声卡和显卡，分辨率为 1024x768 的 VGA 显示器。

----

## 下载镜像文件

!!! attention "注意"

    用于离线安装的 ISO 镜像体积较为庞大，建议采用 [aria2] 和 [Motrix] 之类的下载管理器下载 ISO 镜像文件以避免常规浏览器下载出现网络中断的情况。

[aria2]: https://aria2.github.io/
[Motrix]: https://motrix.app/

### 直接下载

对于 openSUSE（ISO 文件大小约 4.5GB）：

```
aria2c https://opentuna.cn/opensuse/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso
```
```
aria2c https://opentuna.cn/opensuse/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso.sha256
```
```
aria2c https://opentuna.cn/opensuse/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-Current.iso.sha256.asc
```

有关 aria2 的部署，另见 [aria2c]。

[aria2c]: ./../misc/aria2c.md

### 使用 metalink

openSUSE 支持用户以 [metalink] 下载 ISO 文件。最新的 ISO 文件的 `*.meta4` 和 `*sha256` 文件可以在 [get.opensuse.org] 上下载获取。

[get.opensuse.org]: http://get.opensuse.org/tumbleweed/?type=desktop
[metalink]: https://en.wikipedia.org/wiki/Metalink

`*.asc` 文件可以使用下列命令下载获得：

!!! note "注意"

    请将以下所有示例中的 `[快照序号]` 替换为当前 `*.meta4` 文件名中含有的快照序号。

```
wget https://download.opensuse.org/tumbleweed/iso/openSUSE-Tumbleweed-DVD-x86_64-[快照序号]-Media.iso.sha256.asc
```

## 校验文件

### 下载证书

要校验 ISO 的数字签名，你需要在 [get.opensuse.org] 页面的底部，找到并下载 openSUSE 项目的公钥。该公钥应当是：

```
pub   rsa4096 2022-06-20 [SC] [有效至：2026-06-19]
      AD485664E901B867051AB15F35A2F86E29B700A4
uid             [ 完全 ] openSUSE Project Signing Key <opensuse@opensuse.org>
```

### 安装校验工具

```
sudo zypper in gpg2
```

### 在命令行校验文件

首先，使用下列命令导入刚刚下载的证书文件：

```
gpg --import gpg-pubkey-29b700a4-62b07e22.asc
```

检查是否导入成功：

```
gpg --fingerprint
```

将刚刚下载的下列文件放置在同一级文件夹中：

- `openSUSE-Tumbleweed-DVD-x86_64-[快照序号]-Media.iso`
- `openSUSE-Tumbleweed-DVD-x86_64-[快照序号]-Media.iso.sha256`
- `openSUSE-Tumbleweed-DVD-x86_64-[快照序号]-Media.iso.sha256.asc`

然后使用下列命令校验文件的 hash 值：

```
sha256sum -c openSUSE-Tumbleweed-DVD-x86_64-[快照序号]-Media.iso.sha256
```

如果校验和匹配，那么在输出中你会看到这样一行字：

```
openSUSE-Tumbleweed-DVD-x86_64-[快照序号]-Media.iso: OK
```

如果没有出现上述结构，说明 ISO 文件已损坏，需要重新下载。

然后使用下列命令校验签名：

```
gpg --verify openSUSE-Tumbleweed-DVD-x86_64-[快照序号]-Media.iso.sha256.asc
```

如果校验不成功，说明 ISO 文件可能被篡改或者你使用的 asc 文件与 ISO 不匹配。

### 完整流程的操作示例

```
poplar@c004-h0:~/下载> ls -lat
总计 4552872
drwxr-xr-x  5 poplar poplar       4096  8月14日 20:46 .
-rw-r--r--  1 poplar poplar 4661968896  8月14日 20:46 openSUSE-Tumbleweed-DVD-x86_64-Snapshot20230812-Media.iso
drwx------ 24 poplar poplar       4096  8月14日 20:00 ..
-rw-r--r--  1 poplar poplar       1687  8月14日 19:55 gpg-pubkey-29b700a4-62b07e22.asc
-rw-r--r--  1 poplar poplar        827  8月14日 18:55 openSUSE-Tumbleweed-DVD-x86_64-Snapshot20230812-Media.iso.sha256.asc
-rw-r--r--  1 poplar poplar        124  8月14日 18:54 openSUSE-Tumbleweed-DVD-x86_64-Snapshot20230812-Media.iso.sha256
-rw-r--r--  1 poplar poplar      70198  8月14日 18:51 e8884cfd8adc12926844a653bcebce97c8debec7.meta4
-rw-r--r--  1 poplar poplar      70198  8月14日 18:50 openSUSE-Tumbleweed-DVD-x86_64-Snapshot20230812-Media.iso.meta4
drwxr-xr-x  2 poplar poplar          6  8月14日 18:50 Telegram Desktop
drwxr-xr-x  2 poplar poplar        294  8月11日 22:30 ISOs
drwxr-xr-x  5 poplar poplar       4096  7月28日 16:34 Others
poplar@c004-h0:~/下载> sha256sum -c openSUSE-Tumbleweed-DVD-x86_64-Snapshot20230812-Media.iso.sha256
openSUSE-Tumbleweed-DVD-x86_64-Snapshot20230812-Media.iso: 成功
poplar@c004-h0:~/下载> gpg --import gpg-pubkey-29b700a4-62b07e22.asc
gpg: 密钥 35A2F86E29B700A4：公钥 “openSUSE Project Signing Key <opensuse@opensuse.org>” 已导入gpg: 处理的总数：1
gpg:               已导入：1
poplar@c004-h0:~/下载> gpg --verify openSUSE-Tumbleweed-DVD-x86_64-Snapshot20230812-Media.iso.sha256.asc
gpg: 假定被签名的数据在‘openSUSE-Tumbleweed-DVD-x86_64-Snapshot20230812-Media.iso.sha256’
gpg: 签名建立于 2023年08月13日 星期日 18时17分35秒 CST
gpg:               使用 RSA 密钥 35A2F86E29B700A4
gpg: 完好的签名，来自于 “openSUSE Project Signing Key <opensuse@opensuse.org>” [完全]
```

### 使用 Kleopatra 校验文件

有关于此，详见 [Kleopatra]。

[Kleopatra]: ./../misc/kleopatra.md

## 创建可启动镜像

!!! info "注意"
    如果你是虚拟机中安装系统，可以略过以下内容。

首先，你需要一个储存空间大小在 8GB 或以上的 U 盘。然后你可以使用 [Rufus]、[balenaEtcher] 或 [Fedora Media Writer] 等工具将 ISO 文件刻录到 U 盘中，制成一个可引导启动的 Linux 系统安装介质。

- Rufus  
  将你的 U 盘插入电脑，打开 Rufus，它会自动选择可用的移动存储设备。点击“**选择**”打开要刻录的镜像文件。请确认选择正确的设备，然后点击底端的“**开始**”等待刻录自动完成。
- balenaEtcher  
  将你的 U 盘插入电脑，打开 balenaEtcher，点击最左侧的加号下面的“Flash from file”，选择要写入的ISO镜像。点击中间的磁盘图标下面的“Select target”，选择要写入的设备。点击最右边的“Flash!”按钮，开始写入。等待刻录自动完成。
- Fedora Media Writer  
  插入 U 盘，运行 Fedora Media Writer 选择“自定义镜像”，并选择刚刚下载的 ISO 镜像。请确保选择正确的设备，然后点击 “写入磁盘”，等待刻录完成后即可。

如果你想提高 U 盘的利用率或想一个 U 盘容纳多个操作系统的安装文件，你可以访问 [ventoy] 了解更多信息。

[Rufus]: https://rufus.ie/zh/
[balenaEtcher]: https://www.balena.io/etcher/
[Fedora Media Writer]: https://getfedora.org/en/workstation/download/
[ventoy]: https://www.ventoy.net/cn/index.html

## 划分未分配的磁盘空间

!!! tip "关于存储空间"

    如果你选择将 Tumbleweed 作为日常使用的系统，它的根目录起码需要 40GB 的空间（因为 openSUSE 默认启用了快照功能）。

如果要在实体机上安装 Linux，请提前用磁盘分区工具划分一个大小为 20GB 或更大的未分配的磁盘空间（不要格式化和写入文件系统）。

## 引导启动

你需要使用在线搜素引擎或厂商的在线售后网站查询一下你当前的设备如何在开机时手动重定向至 BIOS 界面，然后选择引导设备。

然后将电脑关机，插入可启动镜像（安装媒介），开机进入 BIOS 界面，选择你的安装媒介作为引导设备，进入 Linux 的安装引导界面。