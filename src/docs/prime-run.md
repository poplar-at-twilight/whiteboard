# 显卡切换

## NVIDIA PRIME Render Offload

在安装完 NVIDIA 独显驱动后，删除以下软件包：

```
sudo zypper rm -u suse-prime
sudo zypper al suse-prime
```

使用下列命令检查驱动是否运行在 `modesettings mode`：

```
sudo cat /sys/module/nvidia_drm/parameters/modeset
```

终端输出结果应该是 “Y”。

如果不是，打开 YaST，找到并打开 **引导加载器** -> **内核参数(K)**，在 **可选内核命令行参数(P)** 中，填入 `nvidia-drm.modeset=1`，保存设置并退出 YaST。

然后重启系统。

要让应用在 NVIDIA GPU 运行，使用：

```
__NV_PRIME_RENDER_OFFLOAD=1 __VK_LAYER_NV_optimus=NVIDIA_only __GLX_VENDOR_LIBRARY_NAME=nvidia [命令]
```

或者将上述命令写成名为 `prime-run` 的 bash 脚本，放置在 `$PATH` 中：

```
#!/bin/bash
__NV_PRIME_RENDER_OFFLOAD=1 __VK_LAYER_NV_optimus=NVIDIA_only __GLX_VENDOR_LIBRARY_NAME=nvidia "$@"
```

- 相关：[EnvyControl]

[EnvyControl]: https://github.com/bayasdev/envycontrol

## DRI_PRIME

对于 AMD 显卡，使用 `DRI_PRIME` 变量直接切换显卡即可。