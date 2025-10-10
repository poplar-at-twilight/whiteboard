# whiteboard

我的个人在线文档站；主要收纳一些我感兴趣的东西。

- [Cloudflare Page](https://whiteboard-ui8.pages.dev/)

## 如何在本地构建，预览

下载源码：

```
git clone https://github.com/poplar-at-twilight/whiteboard.git && cd whiteboard
```

创建容器：

```
python3 -m venv venv
```

激活容器：

```
source venv/bin/activate
```

安装依赖：

```
pip install mkdocs mkdocs-material
```

运行服务：

```
mkdocs serve
```

然后你就会在 <http://127.0.0.1:8000/> 看到站点预览。