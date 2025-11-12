# `*.gpg`、`*.sig` 和 `*.asc` 文件的区别

签名（signature）可以验证文件的完整性，密钥对（keypair）中的私钥（private keypair）用于生成签名，公钥（public keypair）用于验证签名。

三种文件都是 `gpg2` 生成的签名文件。

- `*.gpg` 和 `*.sig` 都是二进制文件，`*.asc` 是纯文本文件。
- 生成命令：
    - `gpg -s` 或 `gpg --sign` → `*.gpg`
    - `gpg -b` 或 `--detach-sign` → `*.sig`
    - `gpg --clear-sign` → `*.asc`
- `*.gpg` 是二进制的 GPG 公钥文件[^ref1]，含有被签名文件的一份压缩的拷贝
- `*.sig` 是二进制的 GPG 签名文档文件[^ref2]，可含有被签名的文件
- `*.asc` 是可含有封装文档的 ASCII 加密签名，纯文本格式。

[^ref1]: GNU Privacy Guard public keyring file
[^ref2]: GPG signed document file

一个操作实例[^ref3]（签名，加密，解密，提取文件）：

[^ref3]: 参考 [Making and verifying signatures](https://www.gnupg.org/gph/en/manual/x135.html)

```shell
poplar@Greysia:~/Downloads/Aria2> ll
总计 91348KiB
-rw-r--r-- 1 poplar poplar 91346KiB  9月24日 15:39 vm.iso
poplar@Greysia:~/Downloads/Aria2> gpg -s vm.iso
poplar@Greysia:~/Downloads/Aria2> gpg -b vm.iso
poplar@Greysia:~/Downloads/Aria2> gpg --clear-sign vm.iso
gpg: 输入行长度超过 19995 字符
poplar@Greysia:~/Downloads/Aria2> sha256sum vm.iso > vm.iso.sha256
poplar@Greysia:~/Downloads/Aria2> gpg --clear-sign vm.iso.sha256
poplar@Greysia:~/Downloads/Aria2> ll
总计 262924KiB
-rw-r--r-- 1 poplar poplar 91346KiB  9月24日 15:39 vm.iso
-rw-r--r-- 1 poplar poplar 90179KiB 10月 7日 14:50 vm.iso.asc
-rw-r--r-- 1 poplar poplar 81383KiB 10月 7日 14:50 vm.iso.gpg
-rw-r--r-- 1 poplar poplar     1KiB 10月 7日 14:51 vm.iso.sha256
-rw-r--r-- 1 poplar poplar     1KiB 10月 7日 14:51 vm.iso.sha256.asc
-rw-r--r-- 1 poplar poplar     1KiB 10月 7日 14:50 vm.iso.sig
poplar@Greysia:~/Downloads/Aria2> cat vm.iso.sha256.asc
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA512

7716911111580decfb84ec983f10b1beec1a6d6b18ee31f236495703005dcd4d  vm.iso
-----BEGIN PGP SIGNATURE-----

iHUEARYKAB0WIQQrQpu5CY17IY7L1Nr4+uWxAB7+YwUCZwOFBwAKCRD4+uWxAB7+
Y6SfAP4+aYK5q04517RUW61RLDvWOnroXa56qhp+q4k6Ug9k4AEAzJEHVyvFExOn
2eQkaVVIXRvIc+CjCUyljSNFLQrOOwI=
=nLki
-----END PGP SIGNATURE-----
poplar@Greysia:~/Downloads/Aria2> mv vm.iso vms.iso
poplar@Greysia:~/Downloads/Aria2> ll
总计 262924KiB
-rw-r--r-- 1 poplar poplar 90179KiB 10月 7日 14:50 vm.iso.asc
-rw-r--r-- 1 poplar poplar 81383KiB 10月 7日 14:50 vm.iso.gpg
-rw-r--r-- 1 poplar poplar     1KiB 10月 7日 14:51 vm.iso.sha256
-rw-r--r-- 1 poplar poplar     1KiB 10月 7日 14:51 vm.iso.sha256.asc
-rw-r--r-- 1 poplar poplar     1KiB 10月 7日 14:50 vm.iso.sig
-rw-r--r-- 1 poplar poplar 91346KiB  9月24日 15:39 vms.iso
poplar@Greysia:~/Downloads/Aria2> gpg --output vm.iso --decrypt vm.iso.gpg
gpg: 签名建立于 2024年10月07日 星期一 14时50分31秒 CST
gpg:               使用 EDDSA 密钥 2B429BB9098D7B218ECBD4DAF8FAE5B1001EFE63
gpg: 完好的签名，来自于 “White Poplar <poplar.cubic@gmail.com>” [绝对]
poplar@Greysia:~/Downloads/Aria2> ll
总计 354272KiB
-rw------- 1 poplar poplar 91346KiB 10月 7日 14:53 vm.iso
-rw-r--r-- 1 poplar poplar 90179KiB 10月 7日 14:50 vm.iso.asc
-rw-r--r-- 1 poplar poplar 81383KiB 10月 7日 14:50 vm.iso.gpg
-rw-r--r-- 1 poplar poplar     1KiB 10月 7日 14:51 vm.iso.sha256
-rw-r--r-- 1 poplar poplar     1KiB 10月 7日 14:51 vm.iso.sha256.asc
-rw-r--r-- 1 poplar poplar     1KiB 10月 7日 14:50 vm.iso.sig
-rw-r--r-- 1 poplar poplar 91346KiB  9月24日 15:39 vms.iso
poplar@Greysia:~/Downloads/Aria2> gpg --output vm.iso2.sig --sign vm.iso
poplar@Greysia:~/Downloads/Aria2> ll
总计 435656KiB
-rw------- 1 poplar poplar 91346KiB 10月 7日 14:53 vm.iso
-rw-r--r-- 1 poplar poplar 81383KiB 10月 7日 14:55 vm.iso2.sig
-rw-r--r-- 1 poplar poplar 90179KiB 10月 7日 14:50 vm.iso.asc
-rw-r--r-- 1 poplar poplar 81383KiB 10月 7日 14:50 vm.iso.gpg
-rw-r--r-- 1 poplar poplar     1KiB 10月 7日 14:51 vm.iso.sha256
-rw-r--r-- 1 poplar poplar     1KiB 10月 7日 14:51 vm.iso.sha256.asc
-rw-r--r-- 1 poplar poplar     1KiB 10月 7日 14:50 vm.iso.sig
-rw-r--r-- 1 poplar poplar 91346KiB  9月24日 15:39 vms.iso
poplar@Greysia:~/Downloads/Aria2> ll
总计 90180KiB
-rw-r--r-- 1 poplar poplar 90179KiB 10月 7日 14:50 vm.iso.asc
poplar@Greysia:~/Downloads/Aria2> gpg --output vm.iso --decrypt vm.iso.asc
gpg: 签名建立于 2024年10月07日 星期一 14时50分58秒 CST
gpg:               使用 EDDSA 密钥 2B429BB9098D7B218ECBD4DAF8FAE5B1001EFE63
gpg: 完好的签名，来自于 “White Poplar <poplar.cubic@gmail.com>” [绝对]
poplar@Greysia:~/Downloads/Aria2> ll
总计 180300KiB
-rw------- 1 poplar poplar 90120KiB 10月 7日 15:01 vm.iso
-rw-r--r-- 1 poplar poplar 90179KiB 10月 7日 14:50 vm.iso.asc
```