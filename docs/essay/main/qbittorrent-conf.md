---
tags:
  - 杂谈
  - qBittorrent
---

# qBittorrent 配置备忘录

!!! info "说明"

    这是我个人使用的 qBittorrent 配置。  
    相关：[qBittorrent 参数详细设置教程](./../../archives/qbittorrent-confs.md)

## 安装

```
sudo zypper in qbittorrent
```

## 相关文件

配置文件：

- `~/.config/qBittorrent`
- `~/.local/share/qBittorrent`

下载的文件：

```shell
poplar@c004-h1:~> tree /home/bt/network -L 1
/home/bt/network
├── downloads1
├── downloads2
├── misc
├── torrents1
└── torrents2

6 directories, 0 files
```

## 用户界面

- 在菜单栏的 **视图** → **日志** 中，勾选全部类型的日志信息。
- 调整列：
    - 名称
    - 选定大小
    - 进度
    - 状态
    - 做种数
    - 用户
    - 下载速度
    - 上传速度
    - 剩余时间
    - 比率
    - **标签**
    - **本次会话上传**
    - **最近活动**
- 分类：
    - 完整下载：`/home/bt/network/downloads1`（默认分类）
    - 非完整下载：`/home/bt/network/downloads2`
    - 非分类：`/home/bt/network/misc`
- 标签：
    - `1 类`（长期做种）
    - `2 类`（短期做种）
    - `3 类`（临时下载）
    - `电影`
    - `动画`
    - `其他`
    - `特典`
    - `未读`
    - `新番`
    - `音乐`
    - `OVA`

## 软件设置

在默认配置的基础上

### 行为

修改：

- 启动时窗口：隐藏
- 在通知区域显示 qBittorrent
    - 最小化 qBittorrent 到通知区域
    - 关闭 qBittorrent 到通知区域
- 托盘图表样式：单色（深色主题）

### 下载

修改：

- 为所有文件预分配磁盘空间
- 默认 Torrent 管理模式：自动
- 默认保存位置：`/home/bt/network/misc`

### 连接

修改：

- 端口：大于 `10000` 的任意端口
- 全局最大连接数：800
- 每个 torrent 最大连接数：200

不使用：

- `使用我的路由器 UPnP / NAT-PMP 端口转发`（在路由器上另外设置端口转发）
- 全局上传窗口限制
- 每个 torrent 上传窗口限制

### 速度

修改：

- 对 µTP 协议进行速度限制
- 对传送总开销进行速度限制
- 对本地网络用户进行速度限制

### BitTorret

修改：

- 自动附加这些 tracker 到新下载
    - <https://trackerslist.com>

### WebUI

修改：

- Web 用户界面（远程控制）
- 对本地主机上的客户端跳过身份验证

### 高级

修改：

- 网络接口：`wlp6s0`（无线网卡）
- 保存恢复数据的间隔：60 分钟
- 异步 I/O 线程数：64（AMD 7840H）
- 文件池大小：400
- 校验时内存使用扩增量：256 MiB
- 合并读写
- 发送分块上传建议
- 禁止连接到特权端口上的 peer

## 部署 Peerbanhelper

获取 `PeerBanHelper.jar`：

- 下载地址：<https://github.com/Ghost-chu/PeerBanHelper>
- 文档：<https://pbh-btn.github.io/pbh-docs/>

```shell
poplar@c004-h1:~> tree ~/bin/pbh -L 1
/home/poplar/bin/pbh
├── data
├── key.txt
├── latest.log -> /home/poplar/bin/pbh/data/logs/latest.log
├── pbh.service
├── PeerBanHelper.jar
├── PeerBanHelper.jar.old
└── test-start.sh

2 directories, 6 files
```

`test-start.sh`：

```shell
#!/bin/sh
#测试启动
java -Xmx256M -XX:+UseG1GC -XX:+UseStringDeduplication -XX:+ShrinkHeapInSteps -jar PeerBanHelper.jar nogui
```

`pbh.service`：

```shell
[Unit]
Description=Start PeerBanHelper Service
After=multi-user.target

[Service]
ExecStart=/usr/bin/java -Xmx256M -XX:+UseG1GC -XX:+UseStringDeduplication -XX:+ShrinkHeapInSteps -jar PeerBanHelper.jar nogui
Type=simple
WorkingDirectory=/home/poplar/bin/pbh/

[Install]
WantedBy=multi-user.target
```

### 配置文件

使用 `test-start.sh` 启动服务，在 <http://127.0.0.1:8080> 生成 token 并添加 qBittorrent 下载器。然后关闭脚本。

修改 `~/bin/pbh/data/config/config.yml`：

- `language`：改为 `zh_CN`
- `btn`：改为 `true`，并填入 `app-id` 和 `app-secret`
- `ip-database`：填入申请的 `account-id` 和 `license-key`
- `proxy`：将 `setting: 0` 改为 `setting: 2`，并检查服务器地址和端口是否正确。

然后使用 `pbh` 注册并重载 PeerBanHelper 服务。

### 管理脚本

用于管理 PeerBanHelper 服务的脚本 `pbh`：

```shell
#!/bin/sh
#本脚本用于 Peerbanhelper 的日常维护

export PBH_DIR=/home/poplar/bin/pbh
#PeerBanHelper jar 文件的主目录
export PBH_UPDATE_DIR=/home/poplar/Downloads
#PeerBanHelper jar 新版本文件的默认存放路径

printf 'Hi! You are running pbh.sh, which is a script that simplifies the process of managing peerbanhelper with systemctl.\n'
printf 'Please note:\n'
printf '1. You can only enter one character at a time and the entry is not case sensitive.\n'
printf '2. Please edit the script to set the correct PBH_DIR and PBH_UPGRADE_DIR.\n'
printf '3. Downgrading and removing jar files both require the existence of the specified jar file in PBH_DIR\n'
printf '4. The PeerBanHelper.jar.old file will not be deleted, because logically it should be stable (relative to the new version).\n' && echo

while true; do
    printf 'You can use the PeerBanHelper maintenance script for:\n'
    printf 'A - Registering systemd Services\nS - Stop service\nR - Reload service\nU - Update Jar file\nD - Degrade jar file\nL - Read latest log\nC - Clear problematic jar file\nT - Clear terminal output\nq - End script task\n' && echo
    read answer

    if [ "$answer" = "A" ] || [ "$answer" = "a" ]; then
    #注册 systemd 服务
        echo "Make sure that the pbh.service file is in the PBH_DIR file."
        mkdir -p ~/.config/systemd/user
        cp $PBH_DIR/pbh.service ~/.config/systemd/user
        #复制文件
        systemctl daemon-reload --user
        #重载 systemd
        systemctl enable pbh --user
        #设置开机启动
        echo "The PeerBanHelper service is registered and set to start at boot. You can start it by reloading the service."
        printf -- '-%0.s' {1..100} && echo
        #分隔符
    elif [ "$answer" = "S" ] || [ "$answer" = "s" ]; then
    #暂停服务
        systemctl status pbh --user | grep "Active"
        #读取状态
        systemctl stop pbh --user
        #关闭服务
        systemctl status pbh --user | grep "Active"
        #读取状态
        printf -- '-%0.s' {1..100} && echo
    elif [ "$answer" = "R" ] || [ "$answer" = "r" ]; then
    #重载服务
        systemctl restart pbh --user
        #重启服务
        systemctl status pbh --user | grep "Active"
        #读取状态
        printf -- '-%0.s' {1..100} && echo
    elif [ "$answer" = "U" ] || [ "$answer" = "u" ]; then
    #更新服务
        echo "Please make sure the update file is in PBH_UPDATE_DIR folder!"
        systemctl stop pbh --user
        #关闭服务
        systemctl status pbh --user | grep "Active"
        #读取状态
        mv -f $PBH_DIR/PeerBanHelper.jar $PBH_DIR/PeerBanHelper.jar.old
        #强制备份旧文件
        echo "Backup files complete!"
        mv $PBH_UPDATE_DIR/PeerBanHelper.jar $PBH_DIR
        #拷贝新文件
        echo "Update files completed!"
        systemctl restart pbh --user
        #重启服务
        systemctl status pbh --user | grep "Active"
        #读取状态
        printf -- '-%0.s' {1..100} && echo
    elif [ "$answer" = "D" ] || [ "$answer" = "d" ]; then
    #降级更新
        systemctl stop pbh --user
        #关闭服务
        systemctl status pbh --user | grep "Active"
        #读取状态
        mv $PBH_DIR/PeerBanHelper.jar $PBH_DIR/PeerBanHelper.jar.error
        #停用有问题的文件
        mv $PBH_DIR/PeerBanHelper.jar.old $PBH_DIR/PeerBanHelper.jar
        #更换至旧版文件
        echo "Changed to old version files"
        systemctl restart pbh --user
        #重启服务
        systemctl status pbh --user | grep "Active"
        #读取状态
        printf -- '-%0.s' {1..100} && echo
    elif [ "$answer" = "L" ] || [ "$answer" = "l" ]; then
    #读取日志及状态
        tail -n 30 $PBH_DIR/data/logs/latest.log
        #读取最新日志
        echo
        systemctl status pbh --user | grep "Active"
        #读取状态
        printf -- '-%0.s' {1..100} && echo
    elif [ "$answer" = "C" ] || [ "$answer" = "c" ]; then
    #删除有问题的文件
        rm $PBH_DIR/PeerBanHelper.jar.error
        echo "Problematic versions of files have been cleaned up.\n"
        printf -- '-%0.s' {1..100} && echo
    elif [ "$answer" = "T" ] || [ "$answer" = "t" ]; then
    #清理输出
        clear
    elif [ "$answer" = "Q" ] || [ "$answer" = "q" ]; then
    #清理并退出
        clear
        echo "The script task has ended."
        exit
    else
    #重新开始循环
        echo "ERROR! Invalid input."
        continue
    fi
done
```