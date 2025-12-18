# qBittorrent & PeerBanHelper 配置备忘录

> [!IMPORTANT]
> - 这是我个人使用的 qBittorrent 配置。  
> - 本文通过 `systemctl --user` 进行管理服务；另见 [Systemd/User - Archlinux wiki](https://wiki.archlinux.org/title/Systemd/User)。

> [!TIP]
> qbittorrent 的配置可参考 [qBittorrent 参数详细设置教程](https://blog.17lai.site/posts/f6b32521/)

## 安装

```
sudo dnf in qbittorrent
```

### 开机启动

在 KDE 的系统设置中，把 qbittorrent 添加至开机启动的列表中。

## 相关文件

配置文件：

- `~/.config/qBittorrent`
- `~/.local/share/qBittorrent`

下载文件夹：

```bash
poplar@Greysia:~> tree /home/bt/network -L 1
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
    - 缓存：`/home/bt/network/misc`（临时下载）
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
- 使用我的路由器的 UPnP/NAT-PMP 端口转发（当路由器没有将设备直接映射到公网的时候开启）

不使用：

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

将下载好的 `PeerBanHelper*.zip` 放置到 `pbh` 脚本设定的 `$PBH_UP` 文件夹中。

- 下载地址：<https://github.com/Ghost-chu/PeerBanHelper>
- 文档：<https://pbh-btn.github.io/pbh-docs/>

```bash
poplar@Greysia:~> tree ~/bin/pbh -L 1
/home/poplar/bin/pbh
├── data
├── key.txt
├── libraries
└── PeerBanHelper.jar

3 directories, 2 files
```

## 配置文件

使用下文的 `pbh` 安装并启动服务，然后修改 `~/bin/pbh/data/config/config.yml`：

- `server`: 将 `address` 的值设置为 `127.0.0.1`

然后在 <http://127.0.0.1:8080>。生成 token 并添加 qBittorrent 下载器，并修改 `~/bin/pbh/data/config/config.yml`：

- `language`：改为 `zh_CN`
- `btn`：改为 `true`，并填入 `app-id` 和 `app-secret`
    - 将 `config-url` 改为 `https://btn-prod.ghostchu-services.top/ping/config`
- `proxy`：将 `setting: 0` 改为 `setting: 2`，并检查服务器地址和端口是否正确。
- `logger`：将 `hide-finish-log` 改为 `true`

### 集成管理脚本

用于管理 PeerBanHelper 的脚本 `pbh`：

```bash
#!/bin/bash
# 本脚本用于 Peerbanhelper 的日常维护
# 注意，本脚本只适用于直接使用 PeerBanHelper_*.zip 文件进行手动部署的方式

PBH_DIR=$HOME/bin/PeerBanHelper
# PeerBanHelper 主目录
ZIP_DIR=$HOME/Downloads
# PeerBanHelper 更新包文件路径
CACHE_TMP=/tmp/pbh_update
SER_FILE=$HOME/.config/systemd/user/pbh.service
# systemd 服务文件

start_pbh () {
    systemctl start --user pbh
    systemctl status --user pbh | grep "Loaded"
}

stop_pbh () {
    systemctl stop --user pbh
    systemctl status --user pbh | grep "Loaded"
}

enable_pbh () {
    systemctl daemon-reload --user
    systemctl enable --user pbh
    systemctl status --user pbh | grep "Loaded"
}

disable_pbh () {
    systemctl disable --user pbh --now
    systemctl status --user pbh | grep "Active"
    rm $SER_FILE
    systemctl daemon-reload --user
}

update_pbh () {
    mkdir -p $CACHE_TMP
    printf "WARNING:\nThis will delete the files in the $PBH_DIR folder, do you want to continue? (y/n) "
    read -r rm_duble_check
    rm_duble_check_caps="${rm_duble_check^^}"
    if [[ $rm_duble_check_caps = Y ]]; then
        if [[ -n "$(find "$ZIP_DIR" -maxdepth 1 -name "PeerBanHelper*.zip" -print -quit)" ]]; then
            printf "INFO: found_tarball_to_unzip\n"
            mkdir -p $PBH_DIR
            if [[ -d $PBH_DIR/data ]]; then
                printf "INFO: found_data_dir_to_backup\n"
                mv $PBH_DIR/data $CACHE_TMP
                printf "OK: backup_data_dir\n"
                rm -rf $PBH_DIR/*
                unzip -q $ZIP_DIR/PeerBanHelper*.zip -d $CACHE_TMP
                cp -r $CACHE_TMP/PeerBanHelper/* $PBH_DIR
                printf "OK: update_jar_and_libs\n"
                mv $CACHE_TMP/data $PBH_DIR
                printf "OK: restore_data_dir\n"
            else
                printf "INFO: no_found_data_dir_to_backup\n"
                unzip -q $ZIP_DIR/PeerBanHelper*.zip -d $CACHE_TMP
                rm -rf $PBH_DIR/*
                cp -r $CACHE_TMP/PeerBanHelper/* $PBH_DIR
                printf "OK: update_jar_and_libs\n"
            fi
            rm -rf $ZIP_DIR/PeerBanHelper*.zip
            printf "OK: delete_used_tarball\n"
        else
            printf "ERROR: no_found_tarball\n"
        fi
        enable_pbh
    else
        printf "ERROR: update_canceled\n"
    fi
    rm -r $CACHE_TMP
}

####

while true; do
    printf -- '-%0.s' {1..100} && echo
    printf 'You can use the PeerBanHelper maintenance script for:\n\n'
    printf '1 - Install PeerBanHelper\n'
    printf '2 - Start systemd service\n'
    printf '3 - Stop systemd service\n'
    printf '4 - Enable systemd service\n'
    printf '5 - Delete systemd service\n\n'
    printf 'U - Upgrade PeerBanHelper\n'
    printf 'L - Read logs of PeerBanHelper\n\n'
    printf 'C - Clear terminal output\n'
    printf 'Q - Quit\n\n'
    printf '==> '
    
    read answer
    answer_caps_1="${answer^^}"

    # 安装 PeerBanHelper
    if [[ $answer = 1 ]]; then
        # 注册 systemd 文件
        if [[ -f $SER_FILE ]]; then
            printf "INFO: found_systemd_ser_files\n"
        else
            printf "INFO: no_found_systemd_ser_files\n"
            mkdir -p ~/.config/systemd/user
            echo "[Unit]" > $SER_FILE
            echo "Description=Start PeerBanHelper Service" >> $SER_FILE
            echo >> $SER_FILE
            echo "[Service]" >> $SER_FILE
            echo "ExecStart=$(which java) -jar -Xmx512M -XX:+UseG1GC -XX:+UseStringDeduplication -XX:+ShrinkHeapInSteps PeerBanHelper.jar nogui" >> $SER_FILE
            echo "Type=simple" >> $SER_FILE
            echo "WorkingDirectory=$PBH_DIR" >> $SER_FILE
            echo >> $SER_FILE
            echo "[Install]" >> $SER_FILE
            echo "WantedBy=default.target" >> $SER_FILE
            printf "OK: systemd_ser_files_created\n"
        fi
        # 解压 jar 和库
        if [[ -f $PBH_DIR/PeerBanHelper.jar ]] && [[ -d $PBH_DIR/libraries ]]; then
            printf "INFO: found_main_bin\n"
            enable_pbh
        else
            update_pbh
        fi

    elif [[ $answer = 2 ]]; then
        start_pbh
    elif [[ $answer = 3 ]]; then
        stop_pbh
    elif [[ $answer = 4 ]]; then
        enable_pbh
    elif [[ $answer = 5 ]]; then
        disable_pbh
        printf "OK: uninstall_systemd_ser\n"
    
    elif [[ $answer_caps_1 = U ]]; then
        stop_pbh
        update_pbh
        start_pbh

    elif [[ $answer_caps_1 = L ]]; then
    # 读取 Peerbanhelper 日志
        tail -n 30 $PBH_DIR/data/logs/latest.log
        # 读取最新的 30 条日志
        # 也可以使用：
        #journalctl --user -u pbh

    elif [[ $answer_caps_1 = C ]]; then
        clear

    elif [[ $answer_caps_1 = Q ]]; then
        exit

    else
        echo "ERROR: Invalid input!"
        continue

    fi
done
```