# Shell 脚本与配置文件

## AMD GPU

以核显运行应用的环境变量：

```
DRI_PRIME=0
```

以独显运行程序的环境变量：

```
DRI_PRIME=1
```

## bashrc

```shell
## 代理设置

alias pyx='https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890'
alias dnfx='pyx dnf'
# 对 dnf 设置代理
#alias flatpakx="pyx flatpak --user"
# 对 flatpak 使用代理，并增加 --user 标签
alias set-proxy="export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=http://127.0.0.1:7890"
alias unset-proxy="unset http_proxy; unset https_proxy; unset all_proxy"
# 代理变量手动开关
alias steam-proxy="set-proxy; steam"
# 设置代理，并启动 steam
#alias steam-proxy="set-proxy; env STEAM_FORCE_DESKTOPUI_SCALING=1.5 steam"
# 设置代理，并启动 steam（150% 缩放）

## 软件源命令

#alias flatpak-add="set-proxy; flatpak remote-add --if-not-exists --user flathub https://dl.flathub.org/repo/flathub.flatpakrepo"
# 添加 flathub

## shell 自定义

alias ls="ls --block-size=KiB --color=auto"
# 以 KiB 为单位显示文件大小
alias l="ls --block-size=KiB --color=auto -lt"
# 更简短的命令
export PATH=/home/poplar/.local/bin:/home/poplar/bin:/home/poplar/bin/command:/usr/local/bin:/usr/bin:/bin
# 自定义 $PATH 路径
unset GTK_IM_MODULE
unset QT_IM_MODULE
# 针对 wayland 会话的 Fcitx5 环境变量
alias sudo="sudo "
# 对 sudo 后的字符启用别名
alias set-hostname="sudo hostnamectl set-hostname --pretty 'C004-H1' && sudo hostnamectl set-hostname --static Greysia"
# 设置主机名

## 实用命令

alias sha256sum-dir="find . -type f -exec sha256sum {} \; > ../checksum.sha256; mv ../checksum.sha256 .; echo 'Complete calculation!'"
# 自动计算当前文件夹内的全部文件的哈希，并将结果写入 sha256 文件
alias git-repo-clean="git remote prune origin && git repack && git prune-packed && git reflog expire --expire=1.month.ago && git gc --aggressive"
# 清理并压缩 git 仓库
alias pings="ping mirror.nyist.edu.cn -c 6; ping bing.com -c 6; ping 1.1.1.1 -c 6"
# 测试网络连通性
alias venv-setup="python3 -m venv venv"
alias venv="source venv/bin/activate"
# 启动 python 容器环境
#alias furmark='DRI_PRIME=1 $HOME/bin/FurMark_linux64/FurMark_GUI'
# 启动 GPU 压力测试
```

## update-code

```shell
#!/bin/sh
#本脚本用于更新 VScodium

FILE=/home/poplar/Downloads/VSCodium*.tar.gz

if [ -f $FILE ]; then
    mv /home/poplar/bin/codium/data /home/poplar/bin/data
    printf 'Back up data: OK!\n'
    rm -r /home/poplar/bin/codium/*
    printf 'Remove old exc: OK!\n'
    tar -xf /home/poplar/Downloads/VSCodium*.tar.gz -C /home/poplar/bin/codium
    printf 'Software update: OK!\n'
    mv /home/poplar/bin/data /home/poplar/bin/codium/data
    printf 'Data restored: OK!\n'
    rm /home/poplar/Downloads/VSCodium*.tar.gz
    printf 'Clear tarball: OK!\n'

else
    printf 'ERROR: Update tarball no found!\n'
fi
```

## cab

```shell
#!/bin/sh
# 本脚本用于 cbz 文件半自动化打包

WD1=$HOME/Others/repository/img-main-repo/0-tmp1/0
WD2=$HOME/Others/repository/img-main-repo/2-comic-mix/0-cache
WD3=$HOME/Downloads
# 可用工作目录列表

echo ""
printf 'Choose the working dir:\n'
printf 'WD1: 0-tmp1/0\n'
printf 'WD2: 2-comic-mix/0-cache\n'
printf 'WD3: Downloads\n'
printf 'Enter (1/2/3):'

read -r user_choice

default_wd=""

# 手动选择工作目录
case "$user_choice" in
    1)
        default_wd=$WD1
        ;;
    2)
        default_wd=$WD2
        ;;
    3)
        default_wd=$WD3
        ;;
    *)
        default_wd=$WD2
        printf 'Code: set_default_wd_to_wd2\n'
        ;;
esac

cd $default_wd

while true; do

    echo ""
    echo "Current working path: $default_wd"
    printf '1. Initialize\n'
    printf '2. Unzip the zip file\n'
    printf '3  Edit src.md\n'
    printf '4. Calculate the hash value\n'
    printf '5. Pack the cbz file\n'
    printf '6. Change to another wd\n'
    printf '7. Exit(q)\n\n'
    printf 'Please select an operation:'
    read -r user_operation

    zip_file_count=$(find $default_wd -name "*.zip" -print | wc -l)

    # 初始化
    if [ "$user_operation" = "1" ]; then
        printf 'WARNING:\nThis will delete the files in the WD folder, do you want to continue? (y/n) '
        read -r rm_duble_check
        if [ "$rm_duble_check" = "Y" ] || [ "$rm_duble_check" = "y" ]; then
            rm -f sha256sum.txt src.md
            rm -rf ch* main
            touch sha256sum.txt src.md
            printf 'OK: init_completed\n'
        elif [ "$rm_duble_check" = "N" ] || [ "$rm_duble_check" = "n" ]; then
            printf 'INFO: stop_init\n'
        else
            printf 'ERROR: invalid_input\n'
        fi

    # 解压文件
    elif [ "$user_operation" = "2" ]; then
        if [ "$zip_file_count" -gt 1 ]; then
            ls -l
            read -p "Enter the name of the zip file to unzip: " zip_filename
            if [ -f "$zip_filename" ] && [[ "$zip_filename" == *.zip ]]; then
                read -p "Enter the chapter number: " chapter_number
                unar "$zip_filename" -q -D -o ch$chapter_number
                printf 'OK: unzip_tarballs\n'
            else
                printf 'ERROR: tarball_filename_not_match\n'
            fi
        else
            if [ "$zip_file_count" -eq 1 ]; then
                unar *.zip -q -D -o main
                printf 'OK: unzip_single_tarball\n'
            else
                printf 'INFO: no_zipfile_to_extract\n'
            fi
        fi
    
    # 编辑 src.md
    elif [ "$user_operation" = "3" ]; then
        if [ "$zip_file_count" -eq 1 ]; then
            read -p 'Enter author name of comic:' src_author
            read -p 'Enter title of comic:' src_title
            read -p 'Enter src url of comic:' src_url
            echo "- [$src_author - $src_title]($src_url)" > src.md
            printf 'OK: edit_src_md\n'
        else
            #nano src.md
            kwrite src.md
        fi

    # 计算哈希值
    elif [ "$user_operation" = "4" ]; then
        if [ -d main ] && [ -f src.md ]; then
            sha256sum main/* src.md > sha256sum.txt
            printf 'OK: single_hash_calc\n'
        else
            printf 'INFO: file_not_match_when_calc_single_hash\n'
            if [ -d ch1 ] && [ -f src.md ]; then
                sha256sum ch*/* src.md > sha256sum.txt
                printf 'OK: multi_hash_calc\n'
            else
                printf 'INFO: file_not_match_when_calc_multi_hash\n'
            fi
        fi

    # 压缩文件
    elif [ "$user_operation" = "5" ]; then
        if [ -d main ] && [ -f src.md ] && [ -f sha256sum.txt ]; then
            zip -q tmp.zip main/* sha256sum.txt src.md
            mv tmp.zip tmp.cbz
            printf 'OK: build_finished_single\n'
        else
            printf 'ERROR: file_not_match_single\n'
            if [ -d ch1 ] && [ -f src.md ] && [ -f sha256sum.txt ]; then
                zip -q tmp.zip ch*/* sha256sum.txt src.md
                mv tmp.zip tmp.cbz
                printf 'OK: build_finished_multiple\n'
            else
                printf 'ERROR: file_not_match_multiple\n'
            fi
        fi
        if [ -f tmp.cbz ]; then
            read -p 'Enter author name:' author_name
            read -p 'Enter title name:' title_name
            filename="[$author_name] $title_name"
            mv tmp.cbz "$filename".cbz
            ls -l
        else
            printf 'Info: tmp.cbz_no_found\n'
        fi

    elif [ "$user_operation" == "6" ]; then
        printf 'Choose working dir:\n'
        printf 'WD1: 0-tmp1\n'
        printf 'WD2: 2-comic-mix/0-cache\n'
        printf 'WD3: ~/Downloads\n'
        printf 'Enter (1/2/3):'
        read -r new_choice
        case "$new_choice" in
            1)
                default_wd=$WD1
                ;;
            2)
                default_wd=$WD2
                ;;
            3)
                default_wd=$WD3
                ;;
            *)
                default_wd=$WD2
                printf 'ERROR:invalid_input\nINFO: set_default_wd_to_wd2\n'
                ;;
            esac
        cd $default_wd

    elif [ "$user_operation" == "7" ]; then
        printf 'INFO: stop_and_quit\n'
        break
    elif [ "$user_operation" = "Q" ] || [ "$user_operation" = "q" ]; then
        printf 'INFO: stop_and_quit\n'
        break
    else
        printf 'ERROR: invalid_input'
        continue
    fi
done
```

----

## 配置文件

### git

在 `~/.gitconfig` 中，写入：

```
[user]
    name = Poplar at twilight
    email = poplar.cubic@gmail.com
[http]
    proxy = http://127.0.0.1:7890
```

### python

设置代理（`~/.config/pip/pip.conf`）：

```
[global]
proxy=http://localhost:7890
```