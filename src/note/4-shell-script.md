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

```bash
## 代理设置

alias pyx='https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890'
alias dnfx='pyx dnf'
# 对 dnf 设置代理
alias flatpakx="pyx flatpak"
# 对 flatpak 使用代理
alias set-proxy="export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=http://127.0.0.1:7890"
alias unset-proxy="unset http_proxy; unset https_proxy; unset all_proxy"
# 代理变量手动开关
alias steam-proxy="set-proxy; steam"
# 设置代理，并启动 steam
#alias steam-proxy="set-proxy; env STEAM_FORCE_DESKTOPUI_SCALING=1.5 steam"
# 设置代理，并启动 steam（150% 缩放）

## 软件源命令

alias flatpak-add="set-proxy; flatpak remote-add --if-not-exists --user flathub https://dl.flathub.org/repo/flathub.flatpakrepo"
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
alias set-hostname="sudo hostnamectl set-hostname --pretty 'C004-H2' && sudo hostnamectl set-hostname --static Greysia"
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
```

## update-code

```bash
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

```bash
#!/bin/bash
# 本脚本用于 cbz 文件半自动化打包

# 可用工作目录列表
WD1=$HOME/Others/arch_repo/0-tmp_1/0
WD2=$HOME/Others/arch_repo/2-comic-mix-src/0-tmp/
WD3=$HOME/Downloads

# 颜色变量
BLUE='\e[34m'
RED='\e[31m'
GREEN='\e[32m'
YELLOW='\e[33m'

BOLD='\e[1m'
UNDER='\e[4m'
END='\e[0m'

choose_wd () {

    # 清空变量
    src_title=""
    src_author=""
    default_wd=""

    printf '\n'
    printf "${BOLD}Choose the working dir:${END}\n\n"
    printf '    1) WD1: 0-tmp_1/0\n'
    printf '    2) WD2: 2-comic-mix-src/0-tmp\n'
    printf '    3) WD3: Downloads\n\n'
    printf "${UNDER}Enter (1/2/3):${END} "
    read -r user_choice

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
            default_wd=$WD3
            printf "${RED}ERROR: invalid_input${END}\n${BLUE}INFO: set_default_wd_to_wd3${END}\n"
            ;;
    esac

    cd "$default_wd"
}

choose_wd

while true; do

    echo " "
    printf "Current working path: ${UNDER}${default_wd}${END}\n\n"
    printf 'You can:\n'
    printf '    1. Initialize\n'
    printf '    2. Unzip the zip file\n'
    printf '    3  Edit src.md\n'
    printf '    4. Repack the cbz file\n'
    printf '    5. Change to another wd\n'
    printf '    6(q). Stop & Quit\n\n'
    printf "${UNDER}${YELLOW}Please select an operation:${END} "
    read -r user_operation
    
    # 计算当前文件夹压缩包数量
    zip_file_count=$(find "$default_wd" -name "*.zip" -print | wc -l)

    # 初始化
    if [[ $user_operation = 1 ]]; then
        printf "${RED}WARNING:\nThis will delete the files in the WD folder, do you want to continue? (y/n)${END} "
        read -r rm_duble_check
        local_check="${rm_duble_check^^}"
        if [[ $local_check = Y ]]; then
            if [[ -f sha256sum.txt ]] || [[ -f src.md ]]; then
                rm -f sha256sum.txt src.md
                printf "${GREEN}OK: clear_src_ref_files${END}\n"
            else
                printf "${BLUE}INFO: no_found_src_ref_files${END}\n"
            fi
            if [[ -d main ]] || [[ -d ch01 ]]; then
                rm -rf ch* main
                printf "${GREEN}OK: clear_src_file_cache_dir${END}\n"
            else
                printf "${BLUE}INFO: no_found_src_file_cache_dir${END}\n"
            fi
            src_title=""
            src_author=""
            printf "${GREEN}OK: set_src_var_to_null${END}\n"
            printf "${GREEN}OK: init_completed${END}\n"
        elif [[ $local_check = N ]]; then
            printf "${BLUE}INFO: stop_init${END}\n"
        else
            printf "${RED}ERROR: invalid_input${END}\n"
        fi
    
    # 解压文件
    elif [[ $user_operation = 2 ]]; then
        if [[ $zip_file_count -gt 1 ]]; then
            ls -l
            printf "${YELLOW}Enter the name of the zip file to unzip: ${END}"
            read -r zip_filename
            if [[ -f "$zip_filename" ]] && [[ "$zip_filename" == *.zip ]]; then
                printf "${YELLOW}Enter the chapter number: (0~99) ${END}"
                read -r chapter_number
                if [[ "$chapter_number" =~ ^[0-9]+$ ]]; then
                    unar "$zip_filename" -q -D -o ch$chapter_number
                    printf "${GREEN}OK: unzip_tarballs_in_ch$chapter_number${END}\n"
                else
                    printf "${RED}ERROR: invalid_chapter_number_input${END}\n"
                fi
            else
                printf "${RED}ERROR: tarball_filename_not_match${END}\n"
            fi
        else
            if [[ $zip_file_count -eq 1 ]]; then
                single_zip=$(find "$default_wd" -maxdepth 1 -name "*.zip" -print -quit)
                unar "$single_zip" -q -D -o main
                printf "${GREEN}OK: unzip_single_tarball${END}\n"
            else
                printf "${BLUE}INFO: no_zipfile_to_extract${END}\n"
            fi
        fi

    # 编辑 src.md
    elif [[ $user_operation = 3 ]]; then
        if [[ $zip_file_count -eq 1 ]]; then
            printf "${YELLOW}Enter author name of comic: ${END}" 
            read -r src_author
            printf "${YELLOW}Enter title of comic: ${END}"
            read -r src_title
            printf "${YELLOW}Enter src url of comic: ${END}"
            read -r src_url
            echo "- [$src_author - $src_title]($src_url)" > src.md
            printf "${GREEN}OK: refresh src_author & src_title${END}\n"
            printf "${GREEN}OK: edit_src_md${END}\n"
        else
            #nano src.md
            kwrite src.md
        fi

    # 压缩文件
    elif [[ $user_operation = 4 ]]; then
        # 检查变量
        if [[ -z "$src_author" ]] || [[ -z "$src_title" ]]; then
            printf "${YELLOW}WARNING: Author/Title not set. Please enter them now to rename the file.${END}\n"
            printf "${YELLOW}Enter author name of comic: ${END}" 
            read -r src_author
            printf "${YELLOW}Enter title of comic: ${END}"
            read -r src_title
        else
            printf "${GREEN}OK: find_src_var${END}\n"
        fi
        filename="[$src_author] $src_title"
        # 压缩单章文件
        if [[ -d main ]] && [[ -f src.md ]]; then
            sha256sum main/* src.md > sha256sum.txt
            printf "${GREEN}OK: single_hash_calc${END}\n"
            zip -q tmp.zip main/* sha256sum.txt src.md
            mv tmp.zip tmp.cbz
            printf "${GREEN}OK: build_finished_single${END}\n"
            mv tmp.cbz "$filename".cbz
            ls -l
            rm -rf main src.md sha256sum.txt
            printf "${BLUE}INFO: clear_cache_files${END}\n"
        else
            printf "${BLUE}INFO: file_not_match_when_calc_single_hash${END}\n"
            # 压缩多章文件
            if [[ -d ch01 ]] && [[ -f src.md ]]; then
                sha256sum ch*/* src.md > sha256sum.txt
                printf "${GREEN}OK: multi_hash_calc${END}\n"
                zip -q tmp.zip ch*/* sha256sum.txt src.md
                mv tmp.zip tmp.cbz
                printf "${GREEN}OK: build_finished_multi${END}\n"
                mv tmp.cbz "$filename".cbz
                rm -rf ch* src.md sha256sum.txt
                printf "${BLUE}INFO: clear_cache_files${END}\n"
            else
                printf "${BLUE}INFO: file_not_match_when_calc_multi_hash${END}\n"
            fi
        fi

    # 重定向工作目录
    elif [[ $user_operation = 5 ]]; then
        choose_wd

    elif [[ $user_operation = 6 ]]; then
        printf "${BLUE}INFO: stop_and_quit${END}\n"
        break
    elif [[ $user_operation = Q ]] || [[ $user_operation = q ]]; then
        printf "${BLUE}INFO: stop_and_quit${END}\n"
        break
    else
        printf "${RED}ERROR: invalid_input${END}\n"
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