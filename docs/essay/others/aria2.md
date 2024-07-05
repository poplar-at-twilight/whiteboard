---
comments: true
tags:
  - 杂谈
  - aria2
  - Windows
  - Linux
---

# 安装与使用 aria2

## for Windows 10

### 下载

- aria2: <https://github.com/aria2/aria2/releases>
- AriaNg: <https://github.com/mayswind/AriaNg/releases>

将下载完毕的 aria2 和 `AriaNg-*-AllInOne.zip` 放到同一个文件夹下，然后再创建如下几个空的文本文件：

- aria2.conf
- aria2.log
- aria2.session
- HideRun.vbs
- stop-demon.cmd

### 配置文件

#### aria2.conf

- 此配置文件省略了与 BT 相关的配置；
- BT 下载推荐使用 [qBittorrent]，另见：[qBittorrent 配置指南]。

[qBittorrent]: https://github.com/c0re100/qBittorrent-Enhanced-Edition
[qBittorrent 配置指南]: ./../../archives/qbittorrent-confs.md

```shell
# 文件的保存路径(可使用绝对路径或相对路径), 默认: 当前启动位置
dir=D:\Downloads

# 日志文件保存目录
log=D:\Software\Toolkit\aria2\aria2.log

# 启用磁盘缓存, 0 为禁用缓存, 需 1.16 以上版本, 默认:16M
disk-cache=50M

# 文件预分配方式, 能有效降低磁盘碎片, 默认: prealloc
# 预分配所需时间: none < falloc ? trunc < prealloc
# falloc 和 trunc 则需要文件系统和内核支持, 
# NTFS 建议使用 falloc, EXT3/4 建议 trunc, MAC 下需要注释此项
file-allocation=falloc

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
input-file=D:\Software\Toolkit\aria2\aria2.session

# 在 aria2 退出时保存`错误/未完成`的下载任务到会话文件
save-session=D:\Software\Toolkit\aria2\aria2.session

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

#### HideRun.vbs

`HideRun.vbs` 的作用：

- 用于启动 `aria2c.exe` 并指定参数和配置文件路径；
- 可以让 aria2 启动时不会出现终端窗口。

```shell
CreateObject("WScript.Shell").Run "aria2c.exe --conf-path=aria2.conf --async-dns=false",0
```

#### stop-demon.cmd

`stop-demon.cmd` 脚本用于停止 aria2 进程。

```shell
taskkill /im aria2c.exe /t /f
```

### AriaNG

点击解压完毕的 AriaNG HTML 文件，即可在浏览器中打开 AriaNG。

此时，点击 **AriaNG 设置** -> **RPC(localhost:6800)**，然后填入 `aria2.conf` 中已经设置的 Aria2 RPC 密钥。

### 变更 UA

要变更 user-agent，请在新建下载项时，在**选项**中，勾选 **HTTP**，然后在**自定义请求头（header）**中，添加 `User-Agent: <USER_AGENT>`，例如：适用于一些下载百度云文件的脚本所需的自定义 UA：`User-Agent: netdisk`。

### 开机启动

按下 `win` + `R`，输入 `shell:startup`。将 `HideRun.vbs` 的快捷方式拷贝到此处即可。

----

## for Linux

### 安装

```
sudo zypper in aria2
```

### 配置文件

在 `~/bin/aria2` 目录下新建文本文档：

```
poplar@c004-h0:~/bin> tree aria2
aria2
├── aria2.conf
├── aria2.log
├── aria2.service
├── aria2.session
└── ariang.html
```

#### aria2.conf

此处使用的 aria2.conf 文件与上面的文件不同之处在于

```shell
# 文件的保存路径(可使用绝对路径或相对路径), 默认: 当前启动位置
dir=/home/poplar/Downloads/Aria2

# 日志文件保存目录
log=/home/poplar/bin/aria2/aria2.log


# 从会话文件中读取下载任务
input-file=/home/poplar/bin/aria2/aria2.session

# 在 aria2 退出时保存`错误/未完成`的下载任务到会话文件
save-session=/home/poplar/bin/aria2/aria2.session
```

#### systemd 服务

使用 systemd 服务启动 aria2：

```
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

将 `aria2.service` 文件放置到 `/etc/systemd/system` 文件夹下，然后使用

```
sudo systemctl daemon-reload
sudo systemctl enable aria2 --now
```

### 日志清理

在 `~/bin/command` 下新建一个名为 `aria2-clean` 的 shell 脚本：

```shell
#!/bin/sh
touch /home/poplar/Desktop/clean-log.txt
#创建日志文件
sudo systemctl status aria2 | tee -a -p /home/poplar/Desktop/clean-log.txt
#查询状态
sudo systemctl stop aria2
#关闭服务
rm ~/bin/aria2/aria2.log; touch ~/bin/aria2/aria2.log
#清理日志文件
sudo systemctl restart aria2
#重启服务
sudo systemctl status aria2 | tee -a -p /home/poplar/Desktop/clean-log.txt
#查询状态
clear
#清理输出
echo "Log cleanup completed! For details on cleaning task logs, see the files in the desktop folder."
#打印结果
```

----

## 参考资料

- [Aria2+AriaNG 配置指南（Win10 篇）](https://www.higgs.xyz/archives/7/)