---
tags:
  - 杂谈
  - KeePassXC
  - 2FA
---

# 使用 KeePassXC 配置 TOTP

## 起因

之前 GitHub 要求全部的开发者都必须使用 2FA[^1]，我觉得很麻烦（看着说明列表，需要额外安装一个验证程序）。然后就直接把文档库搬到了 gitlab 上。

然后今日[^2]和群友的交流中，了解并尝试了使用 [KeePassXC](https://keepassxc.org/) 的 TOTP 作为 2FA 验证器。此时，我在 GitHub 上已经删库跑路了，~~如果这时再把文档库搬回 GitHub 多少有点反复横跳的感觉……~~

所以写个简记，供被 GitHub 2FA 策略困扰（不想用手机或多装个 APP）的人查阅。

---

## 如何操作

1. 打开 KeePassXC，同时打开 <https://github.com/settings/security>；
2. 点击 `Enable two-factor authentication` 按钮进入设置页面；
3. 在 KeePassXC 程序中，右键单击 GitHub 账户条目，在菜单中选中 `TOTP` -> `设置 TOTP`；
4. 在 GitHub.com 的设置页面中，点击 `enter this secret`，这时你会看到一个标题为 `Your two-factor secret` 的弹窗窗口，拷贝下方的字符串，然后复制到 KeePassXC 之中，确定并保存后，GitHub 账户条目旁会出现一个时钟的图标；
5. 在 KeePassXC 中，点击条目说明页面右上方的时钟图标可以查看当前的 TOTP 密码；
6. 将 KeePassXC 生成的 TOTP 密码复制到 GitHub.com 中；
7. 验证成功后，GitHub 会生成一个名为 `github-recovery-codes.txt` 的恢复代码，请下载保存此文件；
8. 点击确定完成设置流程。

恢复代码文件可以存储至 KeePassXC 数据库之中（`编辑条目` -> `高级` -> `附件`）

[^1]: [Raising the bar for software security: GitHub 2FA begins March 13](https://github.blog/2023-03-09-raising-the-bar-for-software-security-github-2fa-begins-march-13/)
[^2]: 2023 年 3 月 17 日