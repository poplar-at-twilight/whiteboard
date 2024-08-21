---
comments: true
tags:
  - 杂谈
  - python
---

# 嵌入式 python

首先，从 <https://www.python.org/downloads/windows/> 下载 python 的 **Windows embeddable package**。

然后将下载的压缩包（如 `python-3.12.5-embed-amd64.zip`）解压到某个文件夹，如 `D:\software\lib\python`。

## 安装 pip

下载 [get-pip-py] 到相同的文件夹。

[get-pip-py]: https://pip.pypa.io/en/stable/installation/#get-pip-py

然后编辑此文件夹中的 `python312._pth`，将其修改为：

```
python312.zip
.

# Uncomment to run site.main() automatically
import site
```

在终端中安装 pip：

```
D:\software\lib\python\python.exe get-pip.py
```

## 编辑 PATH

在 Windows 的高级系统设置中，向 PATH 变量添加

- `D:\software\lib\python\Scripts`
- `D:\software\lib\python`

然后就能在 vscode 中调用 pip 了:

```
PS D:\myfiles\misc\git\whiteboard> pip --version
pip 24.2 from D:\software\lib\python\Lib\site-packages\pip (python 3.12)
```

----

## 参考

- [codemee - Windows 下製作可攜式 Python 的方法](https://dev.to/codemee/windows-xia-zhi-zuo-ke-xi-shi-python-de-fang-fa-1m05)