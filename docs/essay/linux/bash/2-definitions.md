---
tags:
  - 资料库
  - Linux
  - Bash
---

# 定义

- `blank`：  
空格或制表符（tab character）；
- `builtin`：  
内建于 Bash shell 的命令；
- `control operator`：  
用于执行控制功能的标记（token），包括换行符（newline）以及 `||`、`&&`、`&`、`;`、`;;`、`;&`、`;;&`、`|`、`|&`、`(` 和 `)`。
- `exit status`：  
命令返回给调用者（caller）的值。该值限制为 8 位，因此最大值为 255。
- `field`：  
一种文本单元，是 shell 扩展之一的结果。扩展后，执行命令时，结果字段（resulting fields）将用作命令名称和参数。
- `filename`：  
用于标识文件的字符串（string）
- `job`：  
由管道（pipeline）及其派生的所有进程组成的一组进程，它们都位于同一进程组中。
- `job control`：  
一种让用户可以有选择地停止（挂起）和重新启动（恢复）进程的执行的一种机制。
- `metacharacter`：  
一种当不被引用（quote）时，可用于分隔单词的字符。元字符有：  空格（`space`）、`tab`、换行符（`newline`），以及：  `|`、`&`、`;`、`(`、`)`、`<` 和 `>`。
- `name`：  
仅由字母、数字和下划线（`_`）组成，且以字母或下划线开头的单词。`name` 常用于 shell 变量和函数名，也被成为标识符（`identifier`）。
- `operator`：  
一般指 `control operator` 或 `redirection operator`。操作符至少包含一个未被引用的元字符。
- `process group`：  
一个包含具有相同进程组 ID 的相关进程的集合。
- `process group ID`：  
代表 `process group` 在其生命周期内的唯一标识符。
- `reserved word`：  
对 shell 有特殊意义，保留给 shell 使用的单词。如 `for` 和 `while`。
- `return status`：  
`exit status` 的同义词。
- `signal`：  
一种内核向进程通知系统中发生的事件的机制。
- `special builtin`：  
被 POSIX 标准归类为特殊命令的 shell 内置命令。
- `token`：  
被 shell 视为单个单元的字符序列。它可以是一个单词，也可以是一个操作符。
- `word`：  
被 shell 视为一个单元的字符序列。单词不得包含未加引号的元字符。