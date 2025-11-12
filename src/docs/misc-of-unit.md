# 混乱的计量单位

位元（bit，又称：比特）是信息资讯最小的单位。位元组（Byte，又称为：字节），一个位元组等于 8 位元。

- 位元的单位是：b
- 位元组的单位是：B

上述两个单位使用的国际单位制词头有两种：

- 十进制：如 KB
- 二进制：在词头与单位之间新增一个 i，如 KiB

## 对照表

|值|[公制]/[十进制]|值|[IEC]/[二进制]|
|:---:|:---:|:---:|:---:|
|1000B = 1kB|kilobyte|1024B = 1KiB|kibibyte|
|1000^2^B = 1MB|megabyte|1024^2^B = 1MiB|mebibyte|
|1000^3^B = 1GB|gigabyte|1024^3^B = 1GiB|gibibyte|
|1000^4^B = 1TB|terabyte|1024^4^B = 1TiB|tebibyte|
|1000^5^B = 1PB|petabyte|1024^5^B = 1PiB|pebibyte|
|1000^6^B = 1EB|exabyte|1024^6^B = 1EiB|exbibyte|
|1000^7^B = 1ZB|zettabyte|1024^7^B = 1ZiB|zebibyte|
|1000^8^B = 1YB|yottabyte|1024^8^B = 1YiB|yobibyte|
|1000^9^B = 1RB|ronnabyte|1024^9^B|-|
|1000^10^B = 1QB|quettabyte|1024^10^B|-|

- 来源：[Multiple-byte units]

[公制]: https://en.wikipedia.org/wiki/Metric_prefix
[十进制]: https://en.wikipedia.org/wiki/Decimal
[二进制]: https://en.wikipedia.org/wiki/Binary_prefix
[IEC]: https://en.wikipedia.org/wiki/IEC_80000-13
[Multiple-byte units]: https://en.wikipedia.org/wiki/Byte#Multiple-byte_units

## 常见使用场景

- 十进制：
    - 硬盘/内存制造商标注的硬盘/内存条容量
- 二进制：
    - Linux 系统的内存/存储空间计算
- 位元：
    - 网络带宽

## 部分命令

使用 `ls` 命令列出文件大小时，可以使用 `--block-size=` 的 flag 指定显示的大小，如：

```shell
poplar@Greysia:~/Downloads/ISO> ll openSUSE-Tumbleweed-DVD-x86_64-Snapshot20241001-Media.iso
-rw-r--r-- 1 root root 4594860032 10月 3日 09:24 openSUSE-Tumbleweed-DVD-x86_64-Snapshot20241001-Media.iso
poplar@Greysia:~/Downloads/ISO> ll openSUSE-Tumbleweed-DVD-x86_64-Snapshot20241001-Media.iso --block-size=MiB
-rw-r--r-- 1 root root 4382MiB 10月 3日 09:24 openSUSE-Tumbleweed-DVD-x86_64-Snapshot20241001-Media.iso
```

详见 `man ls`。

又如 `free -m` 的含义就是以 mebibyte 显示当前的内存信息：

```shell
poplar@Greysia:~> sudo free -m
[sudo] root 的密码：
               total        used        free      shared  buff/cache   available
内存：         15201        6875        2356         335        6634        8325
交换：          8191        1757        6434
poplar@Greysia:~> sudo free --mega
               total        used        free      shared  buff/cache   available
内存：         15939        7203        2476         351        6956        8736
交换：          8589        1842        6747
```

详见 `free --help`。