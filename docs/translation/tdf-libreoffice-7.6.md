# LibreOffice 7.6 Community 公告

- 译文信息：
    - 原文： [Announcement of LibreOffice 7.6 Community](https://blog.documentfoundation.org/blog/2023/08/21/libreoffice-7-6-community/)
    - 作者： [Italo Vignoli](https://blog.documentfoundation.org/blog/author/italovignoli/)
    - 许可证： [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/)
    - 译者： 暮光的白杨
    - 日期： 2023-08-21

----

![](./images/2023-08/tdf/LO76_banner-1024x201.png)

2023 年 8 月 21 日，柏林——LibreOffice 7.6 Community 是由志愿者支持的桌面生产力免费办公套件的最新主要版本，也是基于历史版本编号方案（第一位数字表示发布周期，第二位数字表示主要版本）的最后一个版本，现在可从 <https://www.libreoffice.org/download> 下载适用于 Windows（Intel/AMD 和 ARM 处理器）、macOS（Apple 和 Intel 处理器）和 Linux 平台的安装程序。从 2024 年开始，TDF（文档基金会）将采用基于日历的版本编号，因此下一个主要版本将是 2024 年 2 月发布的 LibreOffice 24.2。

LibreOffice 是唯一可与市场领先者逐一比较功能的个人生产力开源办公套件。经过十二年和五个发布周期——代码清理、代码重构、打磨用户界面、扩展到新的硬件和软件平台、优化与 OOXML 的互操作性以支持用户——开发全新功能的难度越来越大，因此大部分功能都是对现有功能的完善或改进。

----

## LibreOffice 7.6 Community 亮点特性

### 总体而言

* 在主视图中使用触摸板时支持缩放手势。
* 支持文档主题以及 ODF 和 OOXML 文档主题定义的导入和导出。
* 对字体处理进行了许多改进，特别是对于从右到左的字体、CJK 和其他亚洲字母。

### Writer

* 插入菜单中的“新建页码向导”，可在页眉/页脚中轻松一步插入页码。
* 格式工具栏中的段落样式下拉菜单显示的是文档中使用的样式列表，而不是可用样式的完整列表。
* 图表可以根据段落样式更灵活地生成，而不仅仅是根据类别或对象名称。
* 参考书目条目可以直接从参考书目表进行编辑，参考书目默认标记超链接到参考书目表中的匹配行。
* 高亮显示已使用的段落和字符样式以及文本中的直接格式。
* 词组检查：现在可接受 Hunspell 和自定义词典的多词词典项目。

### Calc

* 数字格式：导出到 ODF 时，现在支持用"? "表示整数位数，如果是非有效零，则用空格代替，并且现在接受以秒为单位的小数格式，而不截断，如 `[SS].00`。
* 复制到另一个文档的电子表格现在保留用户定义的打印范围。
* 解算器设置与文档一起保存，并且即使未使用页面样式也会导出。
* 支持形状和注释的绘图样式，包括专用的注释样式，可以自定义新注释的默认外观和文本格式。
* 数据透视表的新紧凑布局。
* 自动过滤器支持按颜色排序。按颜色过滤/排序考虑按数字格式设置的颜色。
* “导入文本”对话框（作为 CSV 文件或无格式文本）有一个新选项，可以不检测科学记数法中的数字（仅当“检测特殊数字”关闭时）。

### Impress & Draw

* 用于在查看演示文稿时切换幻灯片的新导航面板（通过标记“幻灯片放映设置”中的复选框来启用该选项）。
* 现在可以在导航器中按从前到后的顺序列出对象，最上面的对象位于列表的顶部。
* 支持 PDFium 导入中的自由文本注释，以及 PDFium 导出中对墨迹、自由文本和多边形/折线注释的支持。
* 修改了自动调整文本缩放算法，使其工作方式与 MS Office 类似。文本缩放现在将空间缩放（段落和行）和缩放字体分开，其中空间缩放可以是 100%、90% 和 80%，字体缩放四舍五入到最接近的点大小。水平间距（项目符号、缩进）不再缩放。
* 对 CJK 和阿拉伯语言的字体管理进行了多项改进。

我们在 [YouTube] 和 [PeerTube] 上提供了总结 LibreOffice 7.6 社区中主要新功能的视频。

[发行说明]中提供了所有新功能的描述。

[YouTube]: https://www.youtube.com/watch?v=AB7TbrkCTSA
[PeerTube]: https://peertube.opencloud.lu/w/6yyK92nUA39dnhyF3JtkK3
[发行说明]: https://wiki.documentfoundation.org/ReleaseNotes/7.6

----

## LibreOffice 7.6 Community 的贡献者

LibreOffice 7.6 Community 的新功能由 148 名贡献者开发：61% 的代码提交来自 TDF 顾问委员会的 3 家公司（Collabora、Red Hat 和 allotropia）或其他组织雇用的 52 名开发人员，15% 来自 7 名 TDF 的开发人员，其余 24% 来自 89 名个人志愿者。

其他 202 名志愿者（代表数百名提供翻译的人员）已致力于 160 种语言的本地化。 LibreOffice 7.6 Community 以 120 种不同的语言版本发布，比任何其他免费或专有软件都多，因此全球有超过 54 亿人可以使用其母语。此外，超过 23 亿人将这 120 种语言中的一种作为第二语言。

----

## LibreOffice for Enterprises

对于企业级部署，TDF 强烈推荐生态系统合作伙伴提供的 LibreOffice Enterprise 系列应用程序（适用于桌面、移动和云），具有大量专用增值功能和其他优势，如 SLA（服务水平协议）。

详见： <https://www.libreoffice.org/download/libreoffice-in-business/>

生态系统合作伙伴为其企业客户开发的每一行代码都在主代码存储库上与社区共享，并改进了 LibreOffice 技术平台。

基于 LibreOffice 技术的产品可用于主要桌面操作系统（Windows、macOS、Linux 和 ChromeOS）、移动平台（Android 和 iOS）以及云。

----

## 迁移至 LibreOffice

TDF 开发了一项迁移协议，以支持企业从专有办公套件迁移到 LibreOffice，该协议基于 LibreOffice Enterprise 系列 LTS（长期支持）版本的部署，以及由提供与专有产品一致的增值解决方案和服务的认证专业人员提供迁移咨询和培训。详见：<https://www.libreoffice.org/get-help/professional-support/>。

事实上，LibreOffice 凭借其成熟的代码库、丰富的功能集、对开放标准的有力支持、出色的兼容性以及来自认证合作伙伴的 LTS 选项，是那些希望重新获得数据控制权并摆脱供应商锁定的企业的理想解决方案。

----

## 与 Microsoft Office 的互操作性

基于 LibreOffice 技术平台在桌面、移动和云上实现个人生产力的高级功能，LibreOffice 7.6 针对与 MS Office 共享文档或从 ​​MS Office 迁移的用户提供了大量改进和新功能。这些用户应定期查看 LibreOffice 的新版本，因为其进步非常快，每个新版本都比前一个版本有显著改进。

一些最重要的改进：

* Writer
    * 对 DOCX 文件中的帧进行了多项修复，包括丢失的帧、应分开的组合帧、应组合的分割帧、重叠帧、忽略父样式、丢失相对定位、错误绝对定位和丢失旋转。
    * DOCX 段落标记的字符属性现在也存储在 ODT 文件中。
    * 多页浮动表的显着处理改进，尤其是从 DOCX/DOC/RTF 导入/导出文件时。
* Calc
    * 修复了将条件格式的单元格边框颜色导出到 XLSX 的问题。

LibreOffice 提供办公套件市场上最高级别的兼容性，对[开放文档格式]（ODF）的原生支持——在安全性和稳健性方面优于专有格式——对 MS Office 文件的卓越支持，以及对大量传统文档格式的过滤，将所有权和控制权交还给用户。

[开放文档格式]: https://en.wikipedia.org/wiki/OpenDocument

Microsoft 文件仍然基于 ISO 于 2008 年弃用的专有格式，而不是基于 ISO 批准的标准，因此它们隐藏了大量人为复杂性。这就造成了 LibreOffice 的处理问题，因为它默认使用真正的开放标准格式（开放文档格式）。

----

## LibreOffice 7.6 Community 的可用性

LibreOffice 7.6 Community 可从 <https://www.libreoffice.org/download/> 获取。LibreOffice 对专有操作系统的最低要求为 Microsoft Windows 7 SP1 和 Apple macOS 10.15。[这里]列出了基于 LibreOffice 技术的 Android 和 iOS 产品。

[这里]: https://www.libreoffice.org/download/android-and-ios/

对于不需要最新功能、更喜欢经过更多测试和错误修复的版本的用户，值得考虑一下 TDF 维护 LibreOffice 7.5 系列，其中包括几个月的向后移植修复。当前版本是 LibreOffice 7.5.5 Community。

TDF 不会为用户提供技术支持，尽管用户可以从用户邮件列表上的志愿者和 [Ask LibreOffice] 网站获取技术支持。

[Ask LibreOffice]: https://ask.libreoffice.org/

LibreOffice 用户、自由软件倡导者和社区成员可通过 <https://www.libreoffice.org/donate> 捐款支持 TDF。

### 新闻资料袋

下载地址： <https://nextcloud.documentfoundation.org/s/k7kZDbx8HTWmZ8N>