---
comments: true
tags:
  - 杂谈
  - qBittorrent
---

# qBittorrent 配置备忘录

这是以反吸血为目的的 qBittorrent 配置备忘录。

- 参考配置另见；
    - [qBittorrent 参数详细设置教程](./../../archives/qbittorrent-confs.md)
    - [libtorrent reference-Settings](https://www.libtorrent.org/reference-Settings.html)

## 安装

使用：[c0re100/qBittorrent-Enhanced-Edition] -  `qbittorrent_enhanced_*_qt6_x64_setup.exe`

[c0re100/qBittorrent-Enhanced-Edition]: https://github.com/c0re100/qBittorrent-Enhanced-Edition/releases

安装完成后，在 qbittorrent 的安装文件中，新建一个名为 `profile` 的文件夹，以便携模式启动 qbittorrent。

## 日志

在菜单栏的 **视图** → **日志** 中，勾选全部类型的日志信息。

## 软件设置

### 行为

![](./images/qbit/conf-1-1.png)

勾选：

- 删除 Torrent 时提示
- 确认 “暂停/继续所有” 操作
- 使用交替的行颜色
- 隐藏为 0 及无穷大的项：总是
- 在 Windows 启动时启动 qBittorrent
- 启动时的窗口状态：隐藏
- 如果退出时由 Torrent 活动则提示确认

![](./images/qbit/conf-1-2.png)

勾选：

- 下载完成并自动退出时提示确认
  - 在通知区域显示 qBittorrent
  - 最小化 qBittorrent 到通知区域
  - 关闭 qBittorrent 到通知区域
- 使用 qBittorrent 打开 .torrent 文件
- 使用 qBittorrent 打开磁力链接
- 检查程序更新

由于当前系统不会因为睡眠而断开网络链接，所以不勾选：

- 下载时禁止系统自动睡眠
- 做种时禁止系统自动睡眠

### 下载

![](./images/qbit/conf-2-1.png)

勾选：

- 显示 Torrent 内容和选项
- 前置 torrent 对话框
- 手动添加 torrent 时询问是否合并 trackers
- 为所有文件预分配磁盘空间
- 启用递归下载对话框

### 连接

![](./images/qbit/conf-3-1.png)

- Peer 连接协议：TCP 和 µTP

勾选：

- 使用我的路由器 UPnP / NAT-PMP 端口转发
- 全局最大连接数：1000
- 每个 torrent 最大连接数：500

启用 IP 过滤

### 速度

![](./images/qbit/conf-4-1.png)

勾选：

- 对 µTP 协议进行速度限制
- 对传送总开销进行速度限制
- 对本地网络用户进行速度限制

### BitTorret

![](./images/qbit/conf-5-1.png)

勾选：

- 启用 DHT（去中心化网络）以找到更多用户
- 启用用户交换（PeX）以找到更多用户
- 启用本地用户发现以找到更多用户
- 加密模式：**允许加密**
- Automatically update public trackers list
    - <https://trackerslist.com>

### WebUI

![](./images/qbit/conf-6-1.png)

勾选：

- Web 用户界面（远程控制）
- 对本地主机上的客户端跳过身份验证
- 启用点击劫持保护
- 启用跨站请求伪造（CSRF）保护
- 启用 Host header 属性验证

### 高级

![](./images/qbit/conf-7-1.png)

修改：

- 保存恢复数据的间隔：60 分钟
  
额外勾选：

- Auto Ban Unknown Peer from China
- Auto Ban Bittorrent Media Player Peer
- 当 IP 或端口更改时重新通知所有 Tracker

![](./images/qbit/conf-7-2.png)

修改：

- 异步 I/O 线程数：64
    - AMD 7840H
- 文件池大小：400
- 校验时内存使用扩增量：1024 MiB
- 磁盘缓存：-1

![](./images/qbit/conf-7-3.png)

额外勾选：

- 合并读写
- 发送分块上传建议

修改：

- µTP-TCP 混合模式策略：按用户比重（抑制 TCP）

![](./images/qbit/conf-7-4.png)

额外勾选：

- 禁止连接到特权端口上的 peer

## IP 过滤列表

要使用 IP 过滤列表，你需要新建一个名为 `ipfilter.dat` 的纯文本文件，并在 **设置** → **连接** → **IP 过滤** 中勾选此文件。

IP 过滤列表的示例：

```
1.180.24.0-1.180.25.255
240e:918:8008:4::0-240e:918:8008:4::ffff
```

### 封禁中国 IP 段

另见：[为了应对运营商考核上下行比率，恩山无线论坛用户和某云盘使用恶意软件从 Bittorrent 网络无限吸血以提高下载量](https://www.v2ex.com/t/1029736)

1. 打开 [IP <-> Country database at LUDOST.NET]，将 `Select template for formating the output of your query` 改为 `interval`；
2. 在 `Input a list of ISO country codes separated by spaces in this field` 中输入 `cn`，按下 Enter 键；
3. 将弹出的新页面保存为 `.dat` 文件。最后在设置中导入此 IP 过滤列表。

[IP <-> Country database at LUDOST.NET]: https://ip.ludost.net/

## 客户端过滤列表

要启用客户端过滤列表，请在 `profile/qBittorrent/data` 目录下新建名为 `peer_blacklist.txt` 的文件。

格式为 `PeerIP 客户端名` 

示例：

```
-DT0001- dt/torrent/v1.00
-DT0001- dt/torrent/v1.01
-DT0001- DT\s0.0.0.1
```

## 辅助封禁工具

目前有两个流行的工具：

- [Simple-Tracker/qBittorrent-ClientBlocker](https://github.com/Simple-Tracker/qBittorrent-ClientBlocker)
- [Ghost-chu/PeerBanHelper](https://github.com/Ghost-chu/PeerBanHelper)

我使用的是 `Ghost-chu/PeerBanHelper`

双击启动 `peerbanhelper-binary.exe`，然后关闭终端窗口。

在 `/data/config` 目录下，打开 `config.yml`，修改配置：

- 隐藏 Transmission 的配置
- 修改 webui 地址
- 删除用户密码（需要启动跳过验证本地客户端的链接）

如下：

```shell
# 客户端设置
client:
  # 名字，可以自己起，会在日志中显示，只能由字母数字横线组成，数字不能打头
  qbittorrent-001:
    # 客户端类型
    # 支持的客户端列表：
    # qBittorrent
    # Transmission
    # 其它也许以后会加
    type: qBittorrent
    # 客户端地址
    endpoint: "http://127.0.0.1:8080"
    # 登录信息（暂不支持 Basic Auth）
    # 用户名
    username: ""
    # 密码
    password: ""
    # Basic Auth - 不知道这是什么的话，请保持默认
    basic-auth:
      user: ""
      pass: ""
    # 验证 SSL 证书有效性
    verify-ssl: true
    # Http 协议版本
    http-version: "HTTP_1_1"
  # transmission-002:
    # type: Transmission
    # endpoint: "http://127.0.0.1:9091"
    # username: "admin"
    # password: "admin"
    # verify-ssl: true
    # http-version: "HTTP_1_1"
# Http 服务器设置
server:
  # 监听端口
  http: 9898
  # 客户端远程 URL 设置
  # Docker 网络请改 host 模式使用或者设置容器端口暴露
  # 当客户端需要与 PBH 通信时，客户端的 URL 会被更改为 http://<address>:<http-port>/<client-api-route>
  address: "127.0.0.1"
  # 在 PBH 需要给下载器传递地址时，将使用此地址传递，请确保此地址最终可被下载器访问，请【不要】以 / 结尾
  prefix: "http://127.0.0.1:9898"
```

保存，然后在 `peerbanhelper-binary.exe` 同一目录中，新建 `HideRun-peerbanhelper.vbs`：

```
CreateObject("WScript.Shell").Run "peerbanhelper-binary.exe",0
```

将 `HideRun-peerbanhelper.vbs` 的快捷方式拷贝到 `shell:startup` 中，以实现开机启动。

Peerbanhelper 的日志文件在 `/data/logs/` 中。

WebUI 地址默认是 <http://localhost:9898/>