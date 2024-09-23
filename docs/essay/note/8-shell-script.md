---
tags:
  - Linux
  - 笔记
---

# Shell 脚本

shell 脚本一般放置在 `~/bin/command`

* 在其他文件中已提及的 shell 脚本不会在此处列出。

## aria2-m

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
        sudo cp $ARIA2_DIR/aria2.service /etc/systemd/system
        echo "The service files have been copied to /etc."
        #拷贝 service 文件
        sudo systemctl daemon-reload
        sudo systemctl enable aria2 --now
        echo "Service started!"
        printf -- '-%0.s' {1..100} && echo
    elif [ "$answer" = "C" ] || [ "$answer" = "c" ]; then
        ls -lh $ARIA2_DIR/aria2.log
        #读取文件大小
        sudo systemctl status aria2 | grep "Active"
        #查询状态
        sudo systemctl stop aria2
        #关闭服务
        rm $ARIA2_DIR/aria2.log; touch $ARIA2_DIR/aria2.log
        echo "Logs cleaned"
        #清理日志文件
        sudo systemctl restart aria2
        #重启服务
        sudo systemctl status aria2 | grep "Active"
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