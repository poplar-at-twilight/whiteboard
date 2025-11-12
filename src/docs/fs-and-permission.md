# 文件系统、文件所有权及文件权限

所有东西都是文件，包括文件夹也是文件的一种。

## 文件系统

Linux 的文件系统树如下：

```shell
poplar@Greysia:~> tree / -L 1
/
├── bin -> usr/bin
├── boot
├── dev
├── etc
├── home
├── lib -> usr/lib
├── lib64 -> usr/lib64
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin -> usr/sbin
├── srv
├── sys
├── tmp
├── usr
└── var

19 directories, 0 files
```

根目录下，常见的文件夹的用途[^ref-fs]：

[^ref-fs]: 参考《Linux Bible, 10th Edition》Part II: Becoming a Linux Power User - Moving Around the Filesystem，第 94，95 页

- `/bin`：存放常用的 Linux 用户命令，如 `ls`、`date`、`chmod` 等。
- `/boot`：存放可引导 Linux 内核、初始化 RAM 磁盘和引导加载器配置文件（GRUB）
- `/dev`：存放代表系统设备访问点的文件。例如终端设备（`tty*`）、硬盘（`hd*` 或 `sd*`）、RAM（`ram*`）和 CD-ROM（`cd*`）等。用户可以通过这些设备文件直接访问设备。
- `/etc`：存放管理配置文件，大多数是可编辑的纯文本文件。
- `/home`：普通用户的主目录。
- `/media`：为自动挂载的设备（特别是可移除的设备，如移动硬盘）提供一个标准挂载点。在 openSUSE 上，这个位置默认在 `/run/media/$USER/`。如果该设备有卷标，则文件夹的名称就是卷标。
- `/lib`：存放 `/bin` 和 `/sbin` 中的应用程序启动系统时所需的共享库。
- `/mnt`：在被 `/media` 取代前，是一个常用的设备挂载点。
- `/misc`：有时用于根据请求自动挂载文件系统的目录。
- `/opt`：存放附加应用软件（一般是用户自行安装的软件）。
- `/proc`：存放有关系统资源的信息。
- `/root`：`root` 用户的主目录。
- `/sbin`：存放管理命令和守护进程。
- `/sys`：存放调整块存储和管理 cgroup 等参数。
- `/tmp`:存放应用使用的临时文件
- `/usr`：'Unix System Resource' 的缩写，存放用户文档、游戏、图形文件（X11）、库文件（lib）以及其他大量在系统启动时不是必须的命令和文件。`/usr` 用于存放安装后不会更改的文件（理论上，`/usr` 可以只读方式挂载）。
- `/var`：存放各种应用使用的数据。`/var` 常用于存放经常变更的文件。

## XFS 与 Btrfs

XFS 与 Btrfs 都是 openSUSE 默认推荐使用的文件系统。openSUSE 推荐用户数据目录使用 XFS，操作系统目录使用 Btrfs。

> Btrfs 和 XFS 代表了满足 Linux 文件系统需求的成熟且互补的选项。Btrfs 提供现代的写时复制功能，例如简单的快照和压缩以及灵活的存储增长。 XFS 专注于为超大容量提供坚如磐石的稳定性和巨大的可扩展性，并具有持续的高吞吐量。<br />
> <em>详细对比另见：[Btrfs vs XFS: An In-Depth Comparison of Linux File Systems](https://thelinuxcode.com/btrfs-vs-xfs-brief-comparison/)</em>

## 文件所有权与文件权限

一个文件面向三种访问者，

- 文件所有者（author，u）
- 文件所有者所在组的其他用户（group，g）
- 其他人（others，o）

有三种权限：

- 可读（Read, r=4）
- 可写（Write, w=2）
- 可执行（Execute, x=1）

### chmod

一个典型的文件及文件夹应当具有如下的权限设置：

```
poplar@Greysia:~/1> l
总计 0
drwxr-xr-x  1 poplar poplar  12  9月24日 13:22 ./
drwx--x---+ 1 poplar poplar 556  9月24日 13:22 ../
drwxr-xr-x  1 poplar poplar   0  9月24日 13:22 2/
-rw-r--r--  1 poplar poplar   0  9月24日 13:22 2.txt
```

第一行表示当前的文件夹，第二行表示父级文件夹，第三和第四行分别是子文件夹和子文件。

`-rw-r--r--` 共有十个字符，第一位字符表示文件的性质（文件/文件夹/符号链接），然后是三个字符一组，依次表示文件所有者、文件所有者所在组的其他用户和其他用户对这个文件的权限。

`chmod` 命令可以使用数字或字母组合更改文件权限。

使用数字组合时，参考上文，对应的权限组合对应的数值，例如：

```
poplar@Greysia:~/1> chmod 777 2.txt; ll
总计 0
drwxr-xr-x 1 poplar poplar 0  9月24日 13:22 2
-rwxrwxrwx 1 poplar poplar 0  9月24日 13:22 2.txt
```

使用字母组合时，使用 `字母 + 加减符号 + 权限` 的形式。

针对所有用户时，使用 `a`（all），或者省略字母。添加权限使用加号，删除权限使用减号。

两边的字母可以多个（同时指定多个对象），例如：

```
poplar@Greysia:~/1> chmod go-wx 2.txt; ll
总计 0
drwxr-xr-x 1 poplar poplar 0  9月24日 13:22 2
-rwxr--r-- 1 poplar poplar 0  9月24日 13:22 2.txt
```

使用 `-R` 标志或者 `*` 可以将操作递归地（recursively）应用到该文件夹的所有文件（包括子文件和子文件夹）。

#### 文件夹的可执行权限

文件夹应当默认具有可执行权限（`drwxr-xr-x`）。文件目录具有的可执行权限允许用户访问目录内的项目，即使用户无法列出目录内容[^ref-sur]。

[^ref-sur]: 参考 [Why must a folder be executable?](https://superuser.com/a/169418)

```
poplar@Greysia:~/1> ll
总计 0
drwxr-xr-x 1 poplar poplar 0  9月24日 13:22 2
-rw-r--r-- 1 poplar poplar 0  9月24日 13:22 2.txt
poplar@Greysia:~/1> mv 2.txt 2
poplar@Greysia:~/1> chmod -x *; l 2
ls: 无法访问 '2/.': 权限不够
ls: 无法访问 '2/..': 权限不够
ls: 无法访问 '2/2.txt': 权限不够
总计 0
d????????? ? ? ? ?              ? ./
d????????? ? ? ? ?              ? ../
-????????? ? ? ? ?              ? 2.txt
```

### chown

`$ chown 用户名:用户组 /文件路径`