---
comments: true
tags:
  - Linux
  - 笔记
---

# Shell 脚本

shell 脚本一般放置在 `~/bin/command`

* 在其他文件中已提及的 shell 脚本不会在此处列出。

## aria2-clean

详见[安装与使用 aria2](./../main/aria2.md)

## check-iso

```shell
#!/bin/sh
#本脚本用于检查 openSUSE ISO 文件完整性

cd /home/poplar/Downloads/Aria2
#移动到默认下载目录
sha256sum -c openSUSE-Tumbleweed-*.iso.sha256
#校验哈希值
gpg --verify openSUSE-Tumbleweed-*.sha256.asc
#校验签名
mv openSUSE*.* /home/poplar/Downloads/ISO
#将 ISO 文件移动到默认目录
```

## service

```shell
#!/bin/sh
#本脚本用于注册 systemd 服务

echo "Registering systemd services for aria2 and pbh..."

sudo cp /home/poplar/bin/aria2/aria2.service /etc/systemd/system
echo "The service files have been copied to /etc/systemd/system."
#拷贝 service 文件

sudo systemctl daemon-reload
sudo systemctl enable aria2 --now
echo "Service started!"
```