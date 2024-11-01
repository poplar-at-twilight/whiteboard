---
tags:
  - 杂谈
  - qBittorrent
---

# qBittorrent & PeerBanHelper 配置备忘录

!!! info "说明"

    - 这是我个人使用的 qBittorrent 配置。  
        - 相关：[qBittorrent 参数详细设置教程](./../../archives/qbittorrent-confs.md)
    - 本文通过 `systemctl --user` 进行管理服务；另见 [Systemd/User - Archlinux wiki](https://wiki.archlinux.org/title/Systemd/User)。

## 安装

```
sudo zypper in qbittorrent
```

### 开机启动

在 KDE 的系统设置中，把 qbittorrent 添加至开机启动的列表中。

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

获取 `PeerBanHelper.jar` 和 `libraries.tar.gz`：

- 下载地址：<https://github.com/Ghost-chu/PeerBanHelper>
- 文档：<https://pbh-btn.github.io/pbh-docs/>

```shell
poplar@c004-h1:~> tree ~/bin/pbh -L 1
/home/poplar/bin/pbh
├── data
├── key.txt
├── latest.log -> /home/poplar/bin/pbh/data/logs/latest.log
├── libraries
├── pbh.service
├── PeerBanHelper.jar
├── qb-nox.service
└── test-start.sh

3 directories, 6 files
```

`test-start.sh`：

```shell
#!/bin/sh
#测试启动
java -Xmx512M -XX:+UseG1GC -XX:+UseStringDeduplication -XX:+ShrinkHeapInSteps -jar PeerBanHelper.jar nogui
```

`pbh.service`：

```shell
[Unit]
Description=Start PeerBanHelper Service

[Service]
ExecStart=/usr/bin/java -jar -Xmx512M -XX:+UseG1GC -XX:+UseStringDeduplication -XX:+ShrinkHeapInSteps PeerBanHelper.jar nogui
Type=simple
WorkingDirectory=/home/poplar/bin/pbh/

[Install]
WantedBy=default.target
```

### 配置文件

使用 `test-start.sh` 启动服务，在 <http://127.0.0.1:8080> 生成 token 并添加 qBittorrent 下载器。然后关闭脚本。

修改 `~/bin/pbh/data/config/config.yml`：

- `language`：改为 `zh_CN`
- `btn`：改为 `true`，并填入 `app-id` 和 `app-secret`
- `ip-database`：填入申请的 `account-id` 和 `license-key`
- `proxy`：将 `setting: 0` 改为 `setting: 2`，并检查服务器地址和端口是否正确。

然后使用 `pbh` 注册并重载 PeerBanHelper 服务。

## 集成管理脚本

用于管理 PeerBanHelper 的脚本 `pbh`：

```shell
#!/bin/sh
#本脚本用于 Peerbanhelper 的日常维护

export PBH_DIR=/home/poplar/bin/pbh
#PeerBanHelper jar 文件的主目录
export PBH_DIR_U=/home/poplar/Downloads
#PeerBanHelper jar 新版本文件的默认存放路径

LOG_FILE=/home/poplar/bin/pbh/data/logs/*.log.gz

while true; do
    printf -- '-%0.s' {1..100} && echo
    printf 'You can use the PeerBanHelper maintenance script for:\n\n'
    printf 'A - Set up systemd services\n'
    printf 'B - Start systemd service\n'
    printf 'C - Stop systemd service\n\n'
    printf 'D - Update PeerBanHelper\n'
    printf 'E - Degrade PeerBanHelper\n'
    printf 'F - Delete unused jar and logs\n'
    printf 'G - Read logs of PeerBanHelper\n\n'
    printf 'H - Clear terminal output\n'
    printf 'Q - Quit\n\n'
    printf '==> '
    read answer

    if [ "$answer" = "A" ] || [ "$answer" = "a" ]; then
    #注册服务
        mkdir -p ~/.config/systemd/user
        if [ -f $PBH_DIR/pbh.service ]; then
            printf 'Found pbh.service file.\n'
            cp $PBH_DIR/pbh.service ~/.config/systemd/user
            systemctl daemon-reload --user
            systemctl enable --user pbh
            systemctl status --user pbh | grep "Loaded"
            #注册 PeerBanHelper
        else
            printf 'ERROR: pbh.service not found!\n'
        fi

    elif [ "$answer" = "B" ] || [ "$answer" = "b" ]; then
    #启动服务
        systemctl start --user pbh
        systemctl status --user pbh | grep "Active"

    elif [ "$answer" = "C" ] || [ "$answer" = "c" ]; then
    #关闭服务
        systemctl stop --user pbh
        systemctl status --user pbh | grep "Active"

    elif [ "$answer" = "D" ] || [ "$answer" = "d" ]; then
    #更新 Peerbanhelper
        systemctl stop --user pbh
        systemctl status --user pbh | grep "Active"
        #服务中止
        if [ -f $PBH_DIR_U/PeerBanHelper*.zip ]; then
            cd $PBH_DIR_U
            7z x PeerBanHelper*.zip
            rm -r $PBH_DIR/libraries
            printf 'Delete old libraries: OK!\n'
            mv $PBH_DIR_U/PeerBanHelper/libraries $PBH_DIR
            printf 'Update libraries: OK!\n'
            #更新库
            mv -f $PBH_DIR/PeerBanHelper.jar $PBH_DIR/PeerBanHelper.jar.old
            printf 'Backup files: OK!\n'
            mv $PBH_DIR_U/PeerBanHelper/PeerBanHelper.jar $PBH_DIR
            printf 'Update files: OK!\n'
            #更新 jar 文件
            systemctl restart --user pbh
            systemctl status --user pbh | grep "Active"
            rm -r $PBH_DIR_U/PeerBanHelper
        else
            printf 'ERROR: PeerBanHelper*.zip not found!\n'
        fi

    elif [ "$answer" = "E" ] || [ "$answer" = "e" ]; then
    #降级更新 Peerbanhelper
        if [ -f $PBH_DIR/PeerBanHelper.jar.old ]; then
            systemctl stop --user pbh
            systemctl status --user pbh | grep "Active"
            #服务中止
            mv -f $PBH_DIR/PeerBanHelper.jar $PBH_DIR/PeerBanHelper.jar.error
            mv -f $PBH_DIR/PeerBanHelper.jar.old $PBH_DIR/PeerBanHelper.jar
            printf 'Downgrade: OK!\n'
            #更换旧版文件
            systemctl restart --user pbh
            systemctl status --user pbh | grep "Active"
        else
            printf 'ERROR: PeerBanHelper.jar.old not found!\n'
        fi

    elif [ "$answer" = "F" ] || [ "$answer" = "f" ]; then
    #删除有问题的旧版文件和日志
        printf 'Start deleting files...\n'
        rm $PBH_DIR/data/logs/*.log.gz
        printf 'Remove archive logs: OK!\n'
        if [ -f $PBH_DIR/PeerBanHelper.jar.error ]; then
        #删除 PeerBanHelper.jar.error
            rm $PBH_DIR/PeerBanHelper.jar.error
            printf 'Remove PeerBanHelper.jar.error: OK!\n'
        else
            printf 'ERROR: PeerBanHelper.jar.error not found!\n'
        fi

    elif [ "$answer" = "G" ] || [ "$answer" = "g" ]; then
    #读取 Peerbanhelper 日志
        tail -n 30 $PBH_DIR/data/logs/latest.log
        #读取最新的 30 条日志
        #也可以使用：
        #journalctl --user -u pbh

    elif [ "$answer" = "H" ] || [ "$answer" = "h" ]; then
    #清理并退出
        clear

    elif [ "$answer" = "Q" ] || [ "$answer" = "q" ]; then
    #退出
        echo "The script task has ended."
        exit

    else
    #重新开始循环
        echo "ERROR: Invalid input!"
        continue

    fi

done
```