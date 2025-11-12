# KVM 备忘录

## 安装

安装软件包：

```
sudo dnf in @virtualization
```

加入用户组：

```shell
sudo usermod -aG libvirt $USER
```

注销重新登录。

## 主宿机文件共享

### Windows

首先，在虚拟机安装完毕后，在虚拟机关机的情况下，打开虚拟机的详情页，点击 **内存**，勾选 **Enable shared memory**。

然后，点击左下角的 **添加新硬件**，选择 **文件系统**，驱动程序保持默认（即 **virtiofs**），将 **源路径** 修改为要与虚拟机共享的文件夹路径，将 **目标路径** 命名为给定的名称，例如 `sharing-folder`，然后点击 **完成**。

然后下载两个驱动文件：

- 最新版的 [winFSP](https://winfsp.dev/rel/)
- 最新稳定版的 [virtio-win-guest-tools.exe](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/)

将下载的文件打包成 ISO 文件，作为一个 SATA CDROM 存储设备添加到虚拟机中。

```shell
mkisofs -o 文件名.iso /文/件/路/径
```

首先安装 `WinFSP` Core，然后安装 `virtio-win-guest-tools.exe`（`virtiofs`）

安装完毕后，按下 `Win + R` 键唤出 CMD，输入 `services.msc`，然后找到并启动 **VirtIO-FS Service**。

### Linux

参考上文配置好虚拟机后，启动虚拟机，确认 `virtiofsd` 已安装。

然后运行：

```shell
mount -t virtiofs mount_tag /mnt/mount/path
```

`mount_tag` 是上文中设定的 目标路径，`/mnt/mount/path` 则是要挂载 `virtiofs` 的路径，例如：

```shell
sudo mount -t virtiofs sharing-folder /mnt #将 sharing-folder 文件夹挂载到 /mnt
```

## 3D 图形加速

首先关闭虚拟机，然后将虚拟机的 **显示协议 Spice** 的 **监听类型** 修改为 **无**，然后勾选下方的 **OpenGL**。

最后修改 **显卡**，将类型更换为 **Virtio**，并勾选 **3D 加速**。

启动虚拟机即可。

注意，如果独显不能稳定渲染，则使用核显进行 3D 加速渲染。

## 常见问题

### 托盘图标

托盘图标可以清晰地显示 KVM 是否正在运行。

要启用托盘图标，需要先打开虚拟机系统管理器，点击菜单栏的 **编辑** > **Preferences**，勾选 **启用系统托盘图标**。

### 鼠标捕获

要停止鼠标捕获至虚拟机，按下`左 Ctrl` + `左 Alt` 键即可。

### 虚拟机全屏显示

虚拟机切换至全屏的按钮在右上方，切换至全屏后，如果要退出全屏，请将鼠标移动至屏幕正上方，会出现一个退出按钮。

### Windows 虚拟机分辨率无法调节

请安装 `virtio-win-guest-tools.exe`