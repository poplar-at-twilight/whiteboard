---
tags:
  - 杂谈
  - aria2
  - Linux
---

# 安装与使用 aria2

## 安装

```
sudo zypper in aria2
```

## 文件目录

在 `~/bin/aria2` 目录下新建文本文档：

```shell
poplar@c004-h1:~> tree ~/bin/aria2
/home/poplar/bin/aria2
├── aria2.conf
├── aria2.log
├── aria2.service
├── aria2.session
└── index.html

1 directory, 5 files
```

### `aria2.conf`

```shell
# 文件的保存路径(可使用绝对路径或相对路径), 默认: 当前启动位置
dir=/home/poplar/Downloads/Aria2

# 日志文件保存目录
log=/home/poplar/bin/aria2/aria2.log

# 启用磁盘缓存, 0 为禁用缓存, 需 1.16 以上版本, 默认:16M
disk-cache=50M

# 文件预分配方式, 能有效降低磁盘碎片, 默认: prealloc
# 预分配所需时间: none < falloc ? trunc < prealloc
# falloc 和 trunc 则需要文件系统和内核支持,
# NTFS 建议使用 falloc, EXT3/4 建议 trunc, MAC 下需要注释此项
#file-allocation=falloc

# 断点续传
continue=true


## 下载连接相关 ##

# 最大同时下载任务数, 运行时可修改, 默认:5
#max-concurrent-downloads=5

# 同一服务器连接数, 添加时可指定, 默认:1
max-connection-per-server=16

# 最小文件分片大小, 添加时可指定, 取值范围1M -1024M, 默认:20M
# 假定 size=10M, 文件为 20MiB 则使用两个来源下载; 文件为 15MiB 则使用一个来源下载
min-split-size=20M

# 单个任务最大线程数, 添加时可指定, 默认:5
split=32

# 整体下载速度限制, 运行时可修改, 默认:0
#max-overall-download-limit=0

# 单个任务下载速度限制, 默认:0
#max-download-limit=0

# 整体上传速度限制, 运行时可修改, 默认:0
#max-overall-upload-limit=0

# 单个任务上传速度限制, 默认:0
#max-upload-limit=0

# 禁用IPv6, 默认:false
#disable-ipv6=true

# 连接超时时间, 默认:60
#timeout=60

# 最大重试次数, 设置为 0 表示不限制重试次数, 默认:5
#max-tries=5

# 设置重试等待的秒数, 默认:0
#retry-wait=0


## 进度保存相关 ##

# 从会话文件中读取下载任务
input-file=/home/poplar/bin/aria2/aria2.session

# 在 aria2 退出时保存`错误/未完成`的下载任务到会话文件
save-session=/home/poplar/bin/aria2/aria2.session

# 定时保存会话, 0 为退出时才保存, 需 1.16.1 以上版本, 默认:0
save-session-interval=120


## RPC相关设置 ##

# 启用 RPC, 默认:false
enable-rpc=true

# 允许所有来源, 默认:false
rpc-allow-origin-all=true

# 允许非外部访问, 默认:false
#rpc-listen-all=true

# 事件轮询方式, 取值:[epoll, kqueue, port, poll, select], 不同系统默认值不同
#event-poll=select

# RPC监听端口, 端口被占用时可以修改, 默认:6800
#rpc-listen-port=6800

# 设置的 RPC 授权令牌, v1.18.4 新增功能, 取代 --rpc-user 和 --rpc-passwd 选项
rpc-secret=poplar

# 是否启用 RPC 服务的 SSL/TLS 加密,
# 启用加密后 RPC 服务需要使用 https 或者 wss 协议连接
#rpc-secure=true

# 在 RPC 服务中启用 SSL/TLS 加密时的证书文件,
# 使用 PEM 格式时，您必须通过 --rpc-private-key 指定私钥
#rpc-certificate=/path/to/certificate.pem

# 在 RPC 服务中启用 SSL/TLS 加密时的私钥文件
#rpc-private-key=/path/to/certificate.key
```

### `aria2.service`

设置使用 systemd 启动 aria2 的服务文件 `aria2.service`：

```shell
[Unit]
Description=Start aria2 daemon
After=multi-user.target

[Service]
ExecStart=/usr/bin/aria2c --conf-path=/home/poplar/bin/aria2/aria2.conf
Type=simple
WorkingDirectory=/home/poplar/bin/aria2

[Install]
WantedBy=multi-user.target
```

### `index.html`

- 下载地址：<https://github.com/mayswind/AriaNg/releases>

## 启动服务

使用 `~/bin/command/aria2-m` 启动服务：

```shell
#!/bin/sh
#本脚本用于维护 aria2 及 ISO 的下载管理

export ISO_DL_DIR=/home/poplar/Downloads/Aria2
export ISO_DIR=/home/poplar/Downloads/ISO
export ARIA2_DIR=/home/poplar/bin/aria2
#设定目录

printf 'You can use this script to verify ISO files, register Aria2 service and clean up aria2 logs.\n'

while true; do
    printf 'V - Verify ISO files\nR - Register Aria2 service\nC - Clean up aria2 logs\nQ - Quit script' && echo
    read answer

    if [ "$answer" = "V" ] || [ "$answer" = "v" ]; then
    #校验 ISO 文件
        cd $ISO_DL_DIR;ls -l && echo
        sha256sum -c openSUSE*.sha256
        gpg --verify openSUSE*.asc
        #验证完整性
        rm $ISO_DIR/openSUSE*.*
        mv $ISO_DL_DIR/openSUSE*.* $ISO_DIR
        #移动文件至默认位置
        echo "ISO files updated!"
        printf -- '-%0.s' {1..100} && echo
        #分隔符
    elif [ "$answer" = "R" ] || [ "$answer" = "r" ]; then
        echo "Registering systemd services for aria2..."
        mkdir -p ~/.config/systemd/user
        cp $ARIA2_DIR/aria2.service ~/.config/systemd/user
        echo "The service files have been copied to systemd/user."
        #拷贝 service 文件
        systemctl daemon-reload --user
        systemctl enable --now --user aria2
        echo "Service started!"
        printf -- '-%0.s' {1..100} && echo
    elif [ "$answer" = "C" ] || [ "$answer" = "c" ]; then
        ls -lh $ARIA2_DIR/aria2.log
        #读取文件大小
        systemctl status aria2 --user | grep "Active"
        #查询状态
        systemctl stop aria2 --user
        #关闭服务
        rm $ARIA2_DIR/aria2.log; touch $ARIA2_DIR/aria2.log
        echo "Logs cleaned"
        #清理日志文件
        systemctl restart aria2 --user
        #重启服务
        systemctl status aria2 --user | grep "Active"
        ls -lh $ARIA2_DIR/aria2.log
        #查询状态
        printf -- '-%0.s' {1..100} && echo
    elif [ "$answer" = "Q" ] || [ "$answer" = "q" ]; then
        clear; exit
    else
    #重新开始循环
        echo "ERROR! Invalid input."
        continue
    fi
done
```

## 补充说明

- 配置不包含 BT 下载功能，相关另见 [qBittorrent 配置备忘录](./qbittorrent-conf.md)。
- aria2 只有在工作时才会读写日志。下载完文件后，记得要清理一下日志文件。

----

## 参考资料

- [Aria2+AriaNG 配置指南（Win10 篇）](https://www.higgs.xyz/archives/7/)