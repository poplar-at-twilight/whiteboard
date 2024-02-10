---
comments: true
tags:
  - 杂谈
  - Pandoc
  - 笔记
---

# Pandoc’s Markdown spec

- 源文：[Pandoc’s Markdown](https://pandoc.org/MANUAL.html?pandocs-markdown#pandocs-markdown)
- 作者：[John MacFarlane](https://github.com/jgm)
- 许可证：  
    - © 2006-2023 John MacFarlane (<jgm@berkeley.edu>). Released under the GPL, version 2 or greater. This software carries no warranty of any kind. (See COPYRIGHT for full copyright and warranty notices.)
    - [许可证副本](./assets/pandoc-COPYRIGHT.txt)
- 译者：暮光的白杨
- 日期：2023-03

----

!!! info "注意"

    - 本文档的中英术语表另见[此处](#terms)。
    - 本文档并非最终版，有许多地方需要校正。

[Pandoc](https://pandoc.org) 能解析 John Gruber 的 Markdown 语法的一个扩展和轻度修改版本。本文档解释了语法，并指出了与原始 Markdown 的不同之处。除非另有说明，否则可以通过使用 `markdown_strict` 格式而不是 `markdown` 来抑制这些差异。你可以启用或禁用扩展以更精细地指定行为。它们将在后文详述。此外，请参阅上面的[扩展](https://pandoc.org/MANUAL.html?pandocs-markdown#extensions)，了解也适用于其他格式的扩展。

## 哲学

Markdown 旨在易于编写，更重要的是易于阅读：

>一个 Markdown 格式的文件应该是可以作为纯文本发布的，而不像被标记了标签或格式化说明的样子。  
— John Gruber

这一原则指导了 pandoc 在寻找表格、脚注和其他扩展的语法方面的决定。

然而，pandoc 的目标在一个方面与 Markdown 的最初目标不同。Markdown 最初是为 HTML 生成而设计的，而 pandoc 是为多种输出格式而设计的。因此，虽然 pandoc 允许嵌入原始 HTML，但它不鼓励这样做，并提供其他非类 HTML 的方式来表示重要的文档元素，如定义列表、表格、数学和脚注。

## 段落

段落（paragraph）是一行或多行文本，后跟一个或多个空行。换行符被视为空格，因此你可以根据需要重排段落。如果你需要硬换行，请在行尾放置两个或更多空格。

**扩展：`escaped_line_breaks`**

反斜杠（`\`）后跟换行符也是硬换行符。**注意：**在多行和网格表的单元格中，这是创建硬换行符的唯一方法，因为单元格中的尾随空格将被忽略。

## 标题

有两种类型的标题：Setext 和 ATX。

### Setext 式标题

setext 风格的标题是一行带有“下划线”的文本。这个“下划线”由 `=` 或 `-` （用于二级标题）符号构成

```
一级标题
========

二级标题
--------
```

标题文本可以包含内联格式，例如强调（请参阅下面的[内联格式](#inline)）。

### ATX 式标题

ATX 风格的标题由一到六个 `#` 符号和一行文本组成，文本后面可以选择任意数量的 `#` 符号。行首的 `#` 数量为标题级别：

```
# 一级标题

## 二级标题
```

与 Setext 式标题相同，ATX 式标题也可以使用内联格式：

```
### 带有[链接](url)和强调的三级**标题**
```

**扩展：`blank_before_header`**

原始的 Markdown 语法不需要在标题前有一个空行。（除了在文档开头的标题之外，）Pandoc 要求标题前要有空行。这样做的原因是 `#` 很容易意外地在一行的开头结束（可能通过换行）。

```
以下是两个演示文本：
#演示文本1、#演示文本2。
```

如上，许多 Markdown 实现不需要 ATX 标题开头的 `#` 和标题文本之间有空格，因此 `#演示文本1` 和 `#演示文本2` 会被错误地解析为两个标题。为此，pandoc 要求标题开头的 `#` 和标题文本之间要有空格。

## 标题标识符 {#identifiers}

另见上文的 [`auto_identifiers` 扩展](https://pandoc.org/MANUAL.html?pandocs-markdown#extension-auto_identifiers)。

**Extension: `header_attributes`**

使用此语法，你可以在标题文本的末尾为标题分配属性。

```
{#identifier .class .class key=value key=value}
```

例如，将标识符 `foo` 分配给以下全部标题。

```
# 一级标题 {#foo}

## 二级标题 ##    {#foo}

演示文本   {#foo}
-------
```

（此语法与 [PHP Markdown Extra](https://michelf.ca/projects/php-markdown/extra/) 兼容。）

请注意，尽管此语法允许分配 class、key 或 value 属性，但使用者通常不会使用所有这些信息。标识符、class、key 或 value 属性用于 HTML 和基于 HTML 的格式，例如 EPUB 和幻灯片。标识符用于 LaTeX、ConTeXt、Textile、Jira 标记和 AsciiDoc 源码中的标签和链接锚点。

即便指定了 `--number-sections`，具有 `unnumbered` 类的标题不会被编号。属性上下文中的单个连字符 (`-`) 等效于 `.unnumbered`，更适合于非英语文档。

所以，以下的两个标题是一样的：

```
# 标题 {-}

# 标题 {.unnumbered}
```

如果除了 `unnumbered` 之外还存在 `unlisted` 的类，则该标题将不会包含在目录中。（目前此功能仅针对某些格式实现，如：基于 LaTeX、HTML、PowerPoint 和 RTF 的格式。）

**Extension: `implicit_header_references`**

使用该扩展，Pandoc 就会为每一个标题定义引用链接。所以，为了链接到下面的这个标题：

```
# Heading identifiers in HTML
```

你可以写成：

```
[Heading identifiers in HTML]

[Heading identifiers in HTML][]

[the section on heading identifiers][heading identifiers in HTML]
```

而不是明确地给出标识符：

```
[Heading identifiers in HTML](#heading-identifiers-in-html)
```

如果有多个标题具有相同的文本，则相应的引用链接将仅链接到第一个，你将需要使用如上所述的显式链接来链接到其他标题。

与常规引用链接一样，这些引用不区分大小写。

显式链接引用定义始终优先于隐式标题引用。因此，在以下示例中，链接将指向 bar，而不是 #foo：

```
# Foo

[foo]: bar

See [foo]
```

## 块引用

Markdown 使用电子邮件惯例来引用文本块。

块引用（block quotation）由一个或多个段落、或其他块元素（如标题、列表或图片等）构成。每行前面都有一个 `>` 字符和一个可选的空格。（`>` 不必从最左边开始，但是缩进不应超过三个空格）。

```
> 演示文本1
> 演示文本2  
>
> 1. 演示文本3
> 2. 演示文本4
```

你可以仅在第一行放置一个 `>` 字符，如：

```
> 演示文本1
演示文本2  

> 1. 演示文本3
2. 演示文本4
```

你可以嵌套块引用：

```
> 演示文本1
> 演示文本2  
>
> > 被嵌套的演示文本3  
> > 被嵌套的演示文本4
```

如果 `>` 字符后跟一个可选空格，则该空格将被视为块引号标记的一部分，而不是内容缩进的一部分。因此，要将缩进代码块放在块引用中，你需要在 `>` 之后再添加五个空格：

```
>     代码
```

**Extension: `blank_before_blockquote`**

原始的 Markdown 语法不需要在块引用之前有一个空行。（除了在文档开头，）Pandoc 要求块引用前要有一个空行。原因是 `>` （可能通过换行）很容易意外地结束在一行的开头。因此，除非使用 `markdown_strict` 格式，否则以下内容不会在 pandoc 中生成嵌套块引用：

```
> 演示文本1  
>> 演示文本2
```

## 逐字（代码）块

### 缩进代码块

缩进四个空格（或一个制表符）的文本块被视为逐字文本（verbatim text）。也就是说，特殊字符不会触发特殊格式，所有空格和换行符都会保留。例如：

```
    if (a > 3) {
      moveShip(5 * gravity, DOWN);
    }
```

初始（四个空格或一个制表符）缩进不被视为逐字文本的一部分，并在输出中被删除。

注意：逐字文本中的空行不需要以四个空格开头。

### 围栏代码块 {#fenced_code_blocks}

**Extension: `fenced_code_blocks`**

除了标准的缩进代码块，pandoc 还支持围栏代码块。这些代码块以一排三个或更多个波浪号（`~`）开头，以一排波浪号结束，结束行必须至少与起始行一样长。这些行之间的所有内容都被视为代码，且不需要缩进：

````
~~~
if (a > 3) {
  moveShip(5 * gravity, DOWN);
}
~~~
````

与常规代码块一样，围栏代码块必须通过空行与周围的文本分隔开。

如果代码本身包含一行波浪号或反引号，只需在开头和结尾使用一排较长的波浪号或反引号：

````````````
~~~~~~~~~
~~~~
code including tildes
~~~~
~~~~~~~~~
````````````

**Extension: `backtick_code_blocks`**

与 `fenced_code_blocks` 相同，但使用反引号 (`) 而不是波浪号 (~)。

**Extension: `fenced_code_attributes`**

或者，您可以使用以下语法将属性附加到围栏或反引号代码块：

````
~~~ {#mycode .haskell .numberLines startFrom="100"}
qsort []     = []
qsort (x:xs) = qsort (filter (< x) xs) ++ [x] ++
               qsort (filter (>= x) xs)
~~~
````

如上，标识符为 `mycode`，类是 `haskell` 和 `numberLines`，有一个值为 100 的 `startFrom` 属性。某些输出格式可以使用此信息进行语法高亮显示。目前，使用此信息的唯一输出格式是 HTML、LaTeX、Docx、Ms 和 PowerPoint。如果你的输出格式和语言支持突出显示，则上面的代码块将突出显示，并带有编号行。（要查看支持哪些语言，请键入 `pandoc --list-highlight-languages`。）否则，上面的代码块将如下所示：

```html
<pre id="mycode" class="haskell numberLines" startFrom="100">
  <code>
  ...
  </code>
</pre>
```

`numberLines`（或 `number-lines`）类将导致代码块的行被编号，从 1 或 `startFrom` 属性的值开始。`lineAnchors`（或 `line-anchors`）类将使这些行成为 HTML 输出中的可点击锚点。

你也可以使用快捷方式标注代码块的语言，例如：

````
```haskell
qsort [] = []
```
````

它等价于

````
``` {.haskell}
qsort [] = []
```
````

此快捷方式可以与属性结合使用：

````
```haskell {.numberLines}
qsort [] = []
```
````

它等价于

````
``` {.haskell .numberLines}
qsort [] = []
```
````

如果 `fenced_code_attributes` 扩展被禁用，但输入包含代码块的类属性，则第一个类属性将在打开的栅栏之后作为逐字文本打印。

要防止所有突出显示，请使用 [`--no-highlight`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--no-highlight) 标志。要设置突出显示样式，请使用 [`--highlight-style`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--highlight-style)。有关突出显示的更多信息，请参阅下面的[语法突出显示](https://pandoc.org/MANUAL.html?pandocs-markdown#syntax-highlighting)。

## 竖线块

**Extension: `line_blocks`**

竖线块是一系列以竖线（`|`）开头后跟空格的行。文本的分隔将保留在输出中，任何前导空格也是如此；否则，这些行将被格式化为 Markdown。这对诗句和地址很有用：

```
| The limerick packs laughs anatomical
| In space that is quite economical.
|    But the good ones I've seen
|    So seldom are clean
| And the clean ones so seldom are comical

| 200 Main St.
| Berkeley, CA 94718
```

如果需要，可以硬换行，但续行必须以空格开头。

```
| The Right Honorable Most Venerable and Righteous Samuel L.
  Constable, Jr.
| 200 Main St.
| Berkeley, CA 94718
```

竖线块的内容允许内联格式（例如强调），但不允许块级格式（例如块引号或列表）。

此语法是从 [reStructuredText](https://docutils.sourceforge.io/docs/ref/rst/introduction.html) 借用的。

## 列表

### 项目列表

项目列表（bullet lists）的列表项以列表项符号（`*`、`+` 或 `-`）开头。例如：

```
* 列表项一
* 列表项二
* 列表项三
```

这将产生一个“紧凑”列表。如果你想要一个“松散”列表，其中每个列表项都被格式化为一个段落，请在列表项之间放置空格，如：

```
* 列表项一

* 列表项二

* 列表项三
```

项目符号不需要与左边距齐平；它们可以缩进一个、两个或三个空格。项目符号后必须跟有空格。

如果后续行与第一行齐平（在项目符号之后），列表项看起来最好，如：

```
* 这是列表的
  第一个列表项
* 这是列表的第二个列表项
```

当然你也可以写成：

```
* 这是列表的
第一个列表项
* 这是列表的第二个列表项
```

### 列表项中的块内容

一个列表项可能包含多个段落和其他块级内容。但是，后续段落之前必须有一个空行并缩进以与列表项标记符号后的第一个非空格内容对齐。

```
  * 第一段内容

    后续

  * 第二段内容，带有一个
    代码块，需要缩进八个空格。

        { code }
```

例外情况：如果列表项符号后跟一个缩进代码块，它必须在列表项符号后 5 个空格开始，则后续段落必须在列表项符号的最后一个字符后两列开始：

```
*     code

  演示文本
```

列表项可能包括其他列表。在这种情况下，列表前的空行是可选的。嵌套列表必须缩进以与包含列表项的列表项符号之后的第一个非空格字符对齐。

```
* fruits
  + apples
    - macintosh
    - red delicious
  + pears
  + peaches
* vegetables
  + broccoli
  + chard
```

### 有序列表

有序列表与项目列表很相似，不同之处在于有序列表以枚举数开头。

在原始 Markdown 中，枚举数是十进制数字，后跟一个句点和一个空格。数字本身被忽略，以下两个列表是相同的：

```
1.  one
2.  two
3.  three
```

```
5.  one
7.  two
1.  three
```

**Extension: `fancy_lists`**

与原始的 Markdown 不同，pandoc 允许用大小写字母、罗马数字和阿拉伯数字标记有序列表项。列表项符号可以括在括号中，或后跟一个右括号或句点（`.`）。它们必须与后面的文本分隔至少一个空格，并且如果列表标记是带句点的大写字母，则至少分隔两个空格[^1]。

fancy_lists 扩展还允许将 `#` 用作有序列表的列表项符号来代替数字：

```
#. one
#. two
```

**Extension: `startnum`**

Pandoc 还注意使用的列表项符号的类型和起始编号，并且在输出格式中尽可能保留这两者。如下，生成的表格中，数字会从 9 开始并后跟一个括号，子列表包含小写罗马数字：

```
 9)  Ninth
10)  Tenth
11)  Eleventh
       i. subone
      ii. subtwo
     iii. subthree
```

每次使用不同类型的列表项符号时，Pandoc 都会启动一个新列表。因此，以下将创建三个列表：

```
(2) Two
(5) Three
1.  Four
*   Five
```

如果需要默认列表项符号，请使用 `#.`：

```
#.  one
#.  two
#.  three
```

**Extension: `task_lists`**

Pandoc 支持使用 GitHub-Flavored Markdown 语法的任务列表。

```
- [ ] 未检查的项目
- [x] 已检查的项目
```

### 定义列表

**Extension: `definition_lists`**

Pandoc 支持使用带有一些扩展的 [PHP Markdown Extra](https://michelf.ca/projects/php-markdown/extra/) 语法的定义列表（Definition lists）[^2]。

```
术语 1

:   术语 1 的定义

带有*内联标记*的术语 2

:   术语 2 的定义

        { 术语 2 的定义的一些演示代码 }

    术语 2 的定义的第二段
```

每个术语（term）必须占一行，可以选择后跟一个空行，并且必须后跟一个或多个定义（ definition）文本。定义以冒号或波浪号开始，可以缩进一个或两个空格。

一个术语可以有多个定义，每个定义可以由一个或多个块元素（段落、代码块、列表等）组成，每个元素缩进四个空格或一个制表符。定义的正文（不包括第一行）应缩进四个空格。但是，与其他 Markdown 列表一样，除非在段落或其他块元素的开头，你可以“懒惰地”省略缩进：

```
术语 1

:  省略了缩进
的定义文本。

    定义文本的第二段。
```

如果你在定义文本前留出空格（如上例所示），则定义文本将被视为一个段落。在某些输出格式中，这意味着术语与定义之间的间距更大。要获得更紧凑的定义列表，请省略定义前的空格，如下：

```
术语 1
  ~ 定义文本 1

术语 2
  ~ 定义文本 2a
  ~ 定义文本 2b
```

注意，定义列表中的列表项之间的空格是必需的。（[`compact_definition_lists` 扩展](https://pandoc.org/MANUAL.html?pandocs-markdown#extension-compact_definition_lists) 可以放宽此要求，但不允许省略缩进的硬换行。）

### 编号示例列表

**Extension: `example_lists`**

特殊的列表项符号 `@` 可用于按顺序编号的示例。

在整个文档中，带有 `@` 标记的第一个列表项将被编号为 “1”，下一个为 “2”，依此类推。编号示例不需要出现在单个列表中；每个使用 @ 的新列表都将占据上次停止的位置。所以，例如：

```
(@)  这个示例将被编号为 (1)。
(@)  这个示例将被编号为 (2)。

示例的说明

(@)  这个示例将被编号为 (3).
```

你可以在文档的其他地方标记和引用编号示例：

```
(@good)  这是一个示例

正如 (@good) 示例所言，……
```

`@` 后跟的标签可以是任何字母数字字符、下划线或连字符的字符串。

注意：示例列表中的连续段落必须始终缩进四个空格，无论列表项符号的长度如何。也就是说，示例列表始终表现得好像设置了 `four_space_rule` 扩展。这是因为示例标签往往很长，并且将内容缩进到标签后的第一个非空格字符会很尴尬。

### 列表的结束

如果您想在列表后放置一个缩进代码块怎么办？

```
-   item one
-   item two

    { my code block }
```

如上，这就麻烦了！在这里，pandoc（像其他 Markdown 实现一样）会将 `{ my code block }` 视为第二项的第二段，而不是代码块。

要在第二项之后“截断”列表，您可以插入一些非缩进的内容，例如 HTML 注释，这不会产生任何格式的可见输出：

```
-   item one
-   item two

<!-- end of list -->

    { my code block }
```

如果你想要两个连续的列表而不是一个大列表，你可以使用相同的技巧：

```
1.  one
2.  two
3.  three

<!-- -->

1.  uno
2.  dos
3.  tres
```

## 水平线

包含三个或更多 `*`、`-` 或 `_` 字符（可选地用空格分隔）的一行文本行会生成一条水平线：

```
*  *  *  *

---------------
```

我们强烈建议用空行将水平线与周围的文本分隔开。如果水平规则后面没有空行，pandoc 可能会尝试将后面的行解释为 YAML 元数据块或表格。

## 表格

你可以使用四种表格。前三种预设使用等宽字体，例如 Courier。第四种可以与按比例间隔的字体一起使用，因为它不需要排列列。

**Extension: `table_captions`**

你可以选择为所有 4 种表格提供说明（caption）（如下例所示）。

说明是以字符串 `Table:`（或 `table:` 或只是 `:`）开头的段落；你可以将它放置在表格之前或之后。表格说明的字符串在输出中会被删除，只保留说明文本。

```
  Right     Left     Center     Default
-------     ------ ----------   -------
     12     12        12            12
    123     123       123          123
      1     1          1             1

Table:  Demonstration of simple table syntax.
```

标题行和表格行必须各占一行。列对齐由标题文本相对于其下方虚线的位置确定[^3]：

- 如果虚线在右侧与标题文本齐平但在左侧超出它，则该列是右对齐的。
- 如果虚线在左侧与标题文本齐平但在右侧超出它，则该列是左对齐的。
- 如果虚线超出标题文本的两侧，则该列居中。
- 如果虚线与两侧的标题文本齐平，则使用默认对齐方式（在大多数情况下，将保留此对齐方式）。

表格必须以空行结尾，或一行破折号后跟一个空行。

可以省略列标题行，前提是使用虚线结束表格。例如：

```
-------     ------ ----------   -------
     12     12        12             12
    123     123       123           123
      1     1          1              1
-------     ------ ----------   -------
```

省略表头行时，列对齐以表体第一行为准。因此，在上表中，列将分别右对齐、左对齐、居中对齐和右对齐。

**Extension: `multiline_tables`**

多行表格允许标题和表格行跨越多行文本（但不支持跨越表格的多列或多行的单元格）。 这是一个例子：

```
-------------------------------------------------------------
 Centered   Default           Right Left
  Header    Aligned         Aligned Aligned
----------- ------- --------------- -------------------------
   First    row                12.0 Example of a row that
                                    spans multiple lines.

  Second    row                 5.0 Here's another one. Note
                                    the blank line between
                                    rows.
-------------------------------------------------------------

Table: Here's the caption. It, too, may span
multiple lines.
```

这些表格与简单的表格相似，但有以下区别：

- 在标题文本之前（除非标题行被省略），它们必须以一行破折号开头。
- 它们必须以一排破折号结尾，然后是一个空行。
- 行之间必须由空行分隔。

在多行表格中，表格解析器会注意列的宽度，并会尝试在输出中重现这些相对宽度。因此，如果你发现输出中的其中一列太窄，请尝试在 Markdown 源码中将其加宽。

```
----------- ------- --------------- -------------------------
   First    row                12.0 Example of a row that
                                    spans multiple lines.

  Second    row                 5.0 Here's another one. Note
                                    the blank line between
                                    rows.
----------- ------- --------------- -------------------------

: Here's a multiline table without a header.
```

多行表格可能只有一行，但该行后面应该跟一个空行（然后是表末尾的破折号行），否则该表格可能被解释为一个简单的表格。

**Extension: `grid_tables`**

网格表如下所示：

```
: Sample grid table.

+---------------+---------------+--------------------+
| Fruit         | Price         | Advantages         |
+===============+===============+====================+
| Bananas       | $1.34         | - built-in wrapper |
|               |               | - bright color     |
+---------------+---------------+--------------------+
| Oranges       | $2.10         | - cures scurvy     |
|               |               | - tasty            |
+---------------+---------------+--------------------+
```

由 `=` 组成的行将表头与表体分开，无表头的表格可以省略该行。网格表的单元格可能包含任意块元素（多个段落、代码块、列表等）。

单元格可以跨越多列或多行：

```
+---------------------+----------+
| Property            | Earth    |
+=============+=======+==========+
|             | min   | -89.2 °C |
| Temperature +-------+----------+
| 1961-1990   | mean  | 14 °C    |
|             +-------+----------+
|             | min   | 56.7 °C  |
+-------------+-------+----------+
```

表头可能包含多行：

```
+---------------------+-----------------------+
| Location            | Temperature 1961-1990 |
|                     | in degree Celsius     |
|                     +-------+-------+-------+
|                     | min   | mean  | max   |
+=====================+=======+=======+=======+
| Antarctica          | -89.2 | N/A   | 19.8  |
+---------------------+-------+-------+-------+
| Earth               | -89.2 | 14    | 56.7  |
+---------------------+-------+-------+-------+
```

对齐方式可以像管道表一样指定，方法是在标题后的分隔线边界处放置冒号：

```
+---------------+---------------+--------------------+
| Right         | Left          | Centered           |
+==============:+:==============+:==================:+
| Bananas       | $1.34         | built-in wrapper   |
+---------------+---------------+--------------------+
```

对于无头表，冒号改为位于顶行：

```
+--------------:+:--------------+:------------------:+
| Right         | Left          | Centered           |
+---------------+---------------+--------------------+
```

你可以通过使用 `=` 而不是 `-` 的分隔线将其括起来来定义表脚：

```
+---------------+---------------+
| Fruit         | Price         |
+===============+===============+
| Bananas       | $1.34         |
+---------------+---------------+
| Oranges       | $2.10         |
+===============+===============+
| Sum           | $3.44         |
+===============+===============+
```

表脚必须始终放在表格的最底部。

你可以使用 Emacs 的表格模式（`M-x table-insert`）轻松创建网格表。

**Extension: `pipe_tables`**

管道表如下所示：

```
| Right | Left | Default | Center |
|------:|:-----|---------|:------:|
|   12  |  12  |    12   |    12  |
|  123  |  123 |   123   |   123  |
|    1  |    1 |     1   |     1  |

  : Demonstration of pipe table syntax.
```

管道表的语法与 [PHP Markdown Extra 表格](https://michelf.ca/projects/php-markdown/extra/#table)相同。开始和结束竖线字符是可选的，但所有列之间都需要竖线。如图所示，冒号表示列对齐。标题不能省略，要获得一个无标题管道表，请将标题单元格留空。

由于竖线表示列边界，列不需要垂直对齐，如上例所示。所以，以下是一个虽然简略但完全合法的管道表：

```
fruit| price
-----|-----:
apple|2.05
pear|1.37
orange|3.09
```

管道表的单元格中不能包含段落、列表等块状元素，也不能跨多行。如果 markdown 源码的任何一行长于列宽（请参阅 [`--columns`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--columns)），那么表格将占据全文宽度并且单元格内容将换行，相对单元格宽度由分隔表头和表体的直线中的破折号数决定。（例如 ---|- 会使第一列为全文宽度的 3/4，第二列为全文宽度的 1/4。）另一方面，如果没有行宽于列宽，则单元格内容不会换行，单元格的大小将根据其内容调整。

注意：pandoc 还可以识别以下可以由 Emacs 的 orgtbl-mode 生成的管道表：

```
| One | Two   |
|-----+-------|
| my  | table |
| is  | nice  |
```

不同之处在于使用 `+` 而不是 `|`。pandoc 不支持其他 orgtbl 功能。特别是，要获得非默认列对齐，你需要像上面那样添加冒号。

## 元数据块

**Extension: `pandoc_title_block`**

如果文件以标题块开头

```
% 标题
% 作者
% 日期
```

它将被解析为书目信息，而不是常规文本。（例如，它将用于独立 LaTeX 或 HTML 输出的标题。）该块可能仅包含标题、标题和作者，或所有三个元素。如果你想包括一个作者但没有标题，或者一个标题和一个日期但没有作者，你需要一个空行：

```
%
% Author

% My title
%
% June 15, 2006
```

标题可能占据多行，但续行必须以前导空格开头，因此：

```
% My title
  on multiple lines
```

如果文档有多个作者，则作者可以放在单独的，以前导空格开头的行中，或用分号分隔，或两者兼而有之。因此，以下所有内容都是相同的：

```
% Author One
  Author Two

% Author One; Author Two

% Author One;
  Author Two
```

日期必须占一行。

所有三个元数据字段都可能包含标准内联格式（斜体、链接、脚注等）。

标题块将始终被解析，但它们只会在选择 [`--standalone`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--standalone)（[`-s`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--standalone)）选项时影响输出。在 HTML 输出中，标题会出现两次：第一次出现在文档头部，这是将出现在浏览器窗口顶部的标题；另一次出现在文档正文的开头。文档头中的标题可以附加一个可选的前缀（[`--title-prefix`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--title-prefix) 或 [`-T`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--title-prefix) 选项）。正文中的标题显示为类为 `title` 的 H1 元素，因此可以使用 CSS 将其隐藏或重新格式化。如果使用 [`-T`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--title-prefix) 指定标题前缀并且文档中没有出现标题块，则标题前缀将单独用作 HTML 标题。

手册页编写者[^4]从标题行中提取标题、手册页章节号以及其他页眉和页脚信息。标题被认为是标题行的第一个词，可以选择以括号中的节号（个位数）结束。(在标题和括号之间不应有空格。)之后的内容被认为是额外的页脚和页眉文本。应该用一个管道字符（`|`）来分隔页脚文本和页眉文本。例如：

```
% PANDOC(1)
```

将产生一个标题为 PANDOC 和第 1 节的手册页。

```
% PANDOC(1) Pandoc User Manuals
```

页脚中还将包含 “Pandoc User Manuals”。

```
% PANDOC(1) Pandoc User Manuals | Version 4.0
```

页眉中还将包含 “Version 4.0”。

**Extension: `yaml_metadata_block`**

[YAML](https://yaml.org/spec/1.2/spec.html) 元数据块是一个有效的 YAML 对象，由顶部的一行三个连字符 (`---`) 和底部的一行三个连字符 (`---`) 或三个点 (`...`) 分隔。首行 `---` 后面不能跟空行。YAML 元数据块可能出现在文档的任何位置，但如果它不在开头，则必须在其前面加上一个空行。

请注意，由于 pandoc 在提供多个输入文件时连接输入文件的方式，你还可以将元数据保存在单独的 YAML 文件中，并将其作为参数与你的 markdown 文件一并传递给 pandoc：

```shell
pandoc chap1.md chap2.md chap3.md metadata.yaml -s -o book.html
```

你只需确保 YAML 文件以 `---` 开头并以 `---` 或 `....` 结尾。或者，您可以使用 [`--metadata-file`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--metadata-file) 选项。但使用这种方法时，你不能从主要的 markdown 输入文档中引用内容（比如脚注）。

元数据将从 YAML 对象的字段中获取，并添加到任何现有的文档元数据中。元数据可以包含列表和对象（任意嵌套），但所有字符串标量都将被解释为 Markdown。名称以下划线结尾的字段将被 pandoc 忽略。（外部处理器可能会赋予它们一个角色。）字段名称不得解释为 YAML 数字或布尔值（因此，例如，`yes`、`True` 和 `15` 不能用作字段名称）。

一个文档可能包含多个元数据块。如果两个元数据块试图设置相同的字段，则 pandoc 将采用第二个块的值。

每个元数据块都作为独立的 YAML 文档在内部处理。例如：这意味着在一个块中定义的任何 YAML 锚点不能在另一个块中引用。

当 pandoc 与 [`-t markdown`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--to) 一起使用来创建 Markdown 文档时，只有在使用 [`-s/--standalone`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--standalone) 选项时才会生成 YAML 元数据块。所有元数据将出现在文档开头的单个块中。

请注意，必须遵循 [YAML](https://yaml.org/spec/1.2/spec.html) 转义规则。因此，如果标题包含冒号，则必须用引号引起来，如果它包含反斜杠转义，则必须确保它不被视为 [YAML 转义序列](https://yaml.org/spec/1.2/spec.html#id2776092)。管道字符（`|`）可以用来开始一个缩进的块，它将被按字面意思解释，不需要转义。当字段包含空行或块级格式时，此表单是必需的：

```yaml
---
title:  'This is the title: it contains a colon'
author:
- Author One
- Author Two
keywords: [nothing, nothingness]
abstract: |
  This is the abstract.

  It consists of two paragraphs.
...
```

`|` 之后的文字块必须相对于包含 `|` 的行缩进。如果不是，则 YAML 将无效并且 pandoc 不会将其解释为元数据。有关管理 YAML 的复杂规则的概述，请参阅[有关 YAML 语法的维基百科词条](https://en.wikipedia.org/wiki/YAML#Syntax)。

模板变量将从元数据中自动设置。因此，在编写 HTML 时，变量 `abstract` 将被设置为 `abstract` 字段中 Markdown 的 HTML 等值：

```html
<p>This is the abstract.</p>
<p>It consists of two paragraphs.</p>
```

变量可以包含任意 YAML 结构，但模板必须匹配此结构。默认模板中的 `author` 变量需要一个简单的列表或字符串，但可以更改以支持更复杂的结构。例如，如果给出了以下组合，则会为作者添加从属关系：

```yaml
---
title: The document title
author:
- name: Author One
  affiliation: University of Somewhere
- name: Author Two
  affiliation: University of Nowhere
...
```

要在上面的示例中使用结构化 `author`，你需要一个自定义模板：

```
$for(author)$
$if(author.name)$
$author.name$$if(author.affiliation)$ ($author.affiliation$)$endif$
$else$
$author$
$endif$
$endfor$
```

你可以使用 `header-includes` 指定要包含在文档页眉中的原始内容；但是，使用 [`raw_attribute` 扩展](https://pandoc.org/MANUAL.html?pandocs-markdown#extension-raw_attribute)将此内容标记为特定输出格式的原始代码很重要，否则它将被解释为 markdown。例如：

````yaml
header-includes:
- |
  ```{=latex}
  \let\oldsection\section
  \renewcommand{\section}[1]{\clearpage\oldsection{#1}}
  ```
````

注意：`yaml_metadata_block` 扩展与 `commonmark` 和 `markdown` 兼容（在 `gfm` 和 `commonmark_x` 中默认启用）。但是，在这些格式中，以下限制适用：

- YAML 元数据块必须出现在文档的开头（并且只能有一个）。如果将多个文件作为 pandoc 的参数给出，则只有第一个可以是 YAML 元数据块。
- YAML 结构的叶节点（leaf nodes）彼此隔离解析，并与文档的其余部分隔离解析。 因此，例如，如果链接定义位于文档中的其他位置，则你不能在这些上下文中使用参考链接。

## 反斜杠转义

**Extension: `all_symbols_escapable`**

除了在代码块或内联代码中，任何以反斜杠开头的标点符号或空格字符都将按字面意思处理，即使它通常会指示格式。例如，如果一个人写：

```
*\*hello\**
```

那么会渲染为：

```html
<em>*hello*</em>
```

而不是

```html
<strong>hello</strong>
```

这条规则比原始的 Markdown 规则更容易记住，后者只允许对以下字符进行反斜杠转义：

```
\`*_{}[]()>#+-.!
```

（但是，如果使用 `markdown_strict` 格式，则将使用原始 Markdown 规则。）

反斜杠转义空格（backslash-escaped space）被解析为不间断空格。在 TeX 输出中，它将显示为 `~`。在 HTML 和 XML 输出中，它将显示为不间断空格 unicode 字符（请注意，它在生成的 HTML 源代码中实际上看起来“不可见”；你仍然可以使用 [`--ascii`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--ascii) 命令行选项使其显示为一个可见的实体）。

反斜杠转义的换行符（backslash-escaped newline，即出现在行尾的反斜杠）被解析为硬换行符。它将在 TeX 输出中显示为 `\\`，在 HTML 中显示为 `<br />`。这是 Markdown 的“隐形”方式[^5]的一个很好的替代方法。

反斜杠转义在逐字上下文中不起作用。

## 内联格式 {#inline}

### 强调

要对*指定文本*设置斜体，请使用单个 `*` 或 `_` 包围它们，如：

```
*演示文本 演示文本*
_演示文本_
*演示文本*
```

要对**指定文本**设置粗体，请使用两个 `*` 或 `_` 包围它们，如：

被空格包围或反斜杠转义的 `*` 或 `_` 字符不会触发强调：

```
* 演示文本 * \*演示文本\*
```

**Extension: intraword_underscores**

因为 `_` 有时会在单词和标识符中使用，所以 pandoc 不会将被字母数字字符包围的 `_` 解释为强调标记。如果只想强调单词的**一部分**，请使用 *：

```
feas*ible*, not feas*able*.
```

要对***指定文本***设置斜体粗体，请使用三个 `*` 或 `_` 包围它们，如：

```
***指定文本***
```

### 高亮

要<mark>高亮指定文本</mark>，请使用 `mark` 类：

```
[高亮文本]{.mark}
```

或者，没有 `bracketed_spans` 扩展（但有 `native_spans`），你可以：

```html
<span class="mark">高亮指定文本</span>
```

这将在 html 输出中生效。

### 删除线

**Extension: `strikeout`**

要对指定的文本设置删除线，请以 `~~` 作为文本的开头和结尾。例如：

```
~~指定的文本~~
```

### 上标和下标

**Extension: `superscript, subscript`**

上标可以通过用 `^` 字符包围上标文本来编写；下标可以通过用 `~` 字符包围下标文本来编写。因此，例如，

```
H~2~0 是水，2^10^ 是 1024。
```

^...^ 或 ~...~ 之间的文本不能包含空格或换行符。如果上标或下标文本包含空格，则这些空格必须用反斜杠转义。（这是为了防止在日常使用 ~ 和 ^ 时出现意外的上标和下标，以及与脚注的不良交互。）因此，如果你想要下标中带有 `a cat` 的字母 `P`，请使用 `P~a\ cat~`，不是 `P~a cat~`。

### 逐字文本

要逐字逐句显示一小段文本，请将其放在反引号内：

```
What is the difference between `>>=` and `>>`?
```

如果逐字文本包含反引号，请使用双反引号：

```
Here is a literal backtick `` ` ``.
```

（开始反引号之后和结束反引号之前的空格将被忽略。）

一般规则是，逐字跨度以一串连续的反引号（可选地后跟一个空格）开始，并以一串相同数量的反引号（可选地前跟一个空格）结束。

请注意，反斜杠转义符（包括其他 Markdown 结构）在逐字上下文中不起作用：

`` 这是一段演示文本：`\*` ``

**Extension: `inline_code_attributes`**

属性可以附加到逐字文本，就像[围栏代码块](#fenced_code_blocks)一样：

```
`<$>`{.haskell}
```

### 下划线

要对指定的文本设置下划线，请 `underline` 类。例如：

```
[带有下划线的演示文本]{.underline}
```

或者，没有 `bracketed_spans` 扩展（但有 `native_spans`），你可以：

```html
<span class="underline">带有下划线的演示文本</span>
```

这将适用于所有支持下划线的输出格式。

### 小型大写字母

要编写小型大写字母，请使用 `smallcaps` 类：

```
[Small caps]{.smallcaps}
```

或者，没有 `bracketed_spans` 扩展，你可以：

```html
<span class="smallcaps">Small caps</span>
```

为了与其他 Markdown 风格兼容，pandoc 还支持 CSS：

```css
<span style="font-variant:small-caps;">Small caps</span>
```

这将适用于所有支持小型大写字母的输出格式。

## 数学

**Extension: `tex_math_dollars`**

两个 `$` 字符之间的任何内容都将被视为 TeX 数学。开头 `$` 的右边必须有一个非空格字符，而结尾 `$` 的左边必须有一个非空格字符，并且后面不能紧跟数字。因此，`$20,000` 和 `$30,000` 不会解析为数学。如果出于某种原因您需要将文本包含在文字 `$` 字符中，请对它们进行反斜杠转义，它们将不会被视为数学定界符。

对于 display math，请使用 `$$` 分隔符。（在这种情况下，定界符可以用空格与公式分隔开。但是开始和结束 `$$` 定界符之间不能有空行。）

TeX 数学将以所有输出格式打印。它的呈现方式取决于输出格式：

- **LaTeX**  
    它将逐字显示在 `\(...\)`（用于内联数学）或 `\[...\]`（用于 display math）中。  
- **Markdown、Emacs Org mode、ConTeXt、ZimWiki**  
    它将逐字逐句地出现在 `$...$`（用于内联数学）或 `$$...$$`（用于 display math）中。  
- **XWiki**  
    它会逐字显示在 `{{formula}}..{{/formula}}` 中。  
- **reStructuredText**  
    它将使用[解释文本角色 `:math:`](https://docutils.sourceforge.io/docs/ref/rst/roles.html#math) 呈现。  
- **AsciiDoc**  
    对于 AsciiDoc 输出格式（[`-t asciidoc`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--to)），它将逐字逐句地被 `latexmath:[$...$]` 包围（用于内联数学）或被 `[latexmath]++++\[...\]+++` 包围（用于 display math）。对于 AsciiDoctor 输出格式（[`-t asciidoctor`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--to)），LaTeX 定界符（`$..$` 和 `\[..\]`）被省略。  
- **Texinfo**  
    它将在 `@math` 命令中呈现。  
- **roff man、Jira markup**  
    它将在没有 `$` 的情况下逐字呈现。  
- **MediaWiki、DokuWiki**  
    它将在 `<math>` 标签内呈现。  
- **Textile**  
    它将在 `<span class="math">` 标签内呈现。
- **RTF、OpenDocument**  
    如果可能，它将使用 Unicode 字符呈现，否则将逐字显示。  
- **ODT**  
    如果可能，它将使用 MathML 呈现。  
- **DocBook**  
    如果使用 [`--mathml`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--mathml) 标志，它将在 `inlineequation` 或 `informalequation` 标记中使用 MathML 呈现。否则，如果可能，将使用 Unicode 字符呈现。  
- **Docx 和 PowerPoint**  
    它将使用 OMML 数学标记呈现。
- **FictionBook2**  
    如果使用 [`--webtex`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--webtex) 选项，公式将使用 CodeCogs 或其他兼容的网络服务呈现为图像，下载并嵌入电子书中。否则，它们将逐字出现。  
- **HTML、Slidy、DZSlides、S5、EPUB**  
    数学在 HTML 中的呈现方式将取决于所选的命令行选项。因此请参阅上面的 [HTML 中的数学渲染](https://pandoc.org/MANUAL.html?pandocs-markdown#math-rendering-in-html)。

## 原始 HTML

**Extension: `raw_html`**

Markdown 允许您在文档中的任何位置插入原始 HTML（或 DocBook）（逐字上下文除外，其中 <、> 和 & 按字面解释）。（从技术上讲，这不是一个扩展，因为标准 Markdown 允许它，但它已经成为一个扩展，以便在需要时可以将其禁用。）

原始 HTML 在 HTML、S5、Slidy、Slideous、DZSlides、EPUB、Markdown、CommonMark、Emacs Org 模式和 Textile 输出中通过，并在其他格式中被抑制。

有关在 Markdown 文档中包含原始 HTML 的更明确的方法，请参阅 [`raw_attribute` 扩展](https://pandoc.org/MANUAL.html?pandocs-markdown#extension-raw_attribute)。

在 CommonMark 格式中，如果启用了 `raw_html`，则上标、下标、删除线和小型大写字母将以 HTML 表示。否则，将使用纯文本回退。请注意，即使禁用了 `raw_html`，如果表格不能使用管道语法，它们也会使用 HTML 语法呈现。

**Extension: `markdown_in_html_blocks`**

原始 Markdown 允许您包含 HTML “块”：对等标签之间的 HTML 块，这些标签与周围的文本用空行分隔，并在左边距开始和结束。在这些块中，所有内容都被解释为 HTML，而不是 Markdown；例如，* 并不表示强调。

当使用 `markdown_strict` 格式时，Pandoc 的行为是这样的；但默认情况下，pandoc 将 HTML 块标记之间的内容解释为 Markdown。例如，pandoc 将如下内容

```html
<table>
<tr>
<td>*one*</td>
<td>[a link](https://google.com)</td>
</tr>
</table>
```

转换为

```html
<table>
<tr>
<td><em>one</em></td>
<td><a href="https://google.com">a link</a></td>
</tr>
</table>
```

而 `Markdown.pl` 将按原样保留它。

此规则有一个例外：`<script>`、`<style>` 和 `<textarea>` 标签之间的文本不会被解释为 Markdown。

这种与原始 Markdown 的背离使得 pandoc 更容易地将 Markdown 与 HTML 块元素混合使用。例如，你可以用 `<div>` 标签包围一段 Markdown 文本，而不用担心它不会被解释为 Markdown。

**Extension: `native_divs`**

该扩展用于对 `<div>` 标签内的内容使用原生 pandoc `Div` 块。在大多数情况下，这应该提供与 `markdown_in_html_blocks` 相同的输出，但它可以更容易地编写 pandoc 过滤器来操作块组。

**Extension: `native_spans`**

该扩展用于对 `<span>` 标签内的内容使用原生 pandoc `Span` 块。在大多数情况下，这应该提供与 `raw_html` 相同的输出，但它使编写 pandoc 过滤器来操作内联组变得更容易。

**Extension: `raw_tex`**

除了原始 HTML 之外，pandoc 还允许将原始 LaTeX、TeX 和 ConTeXt 包含在文档中。内联 TeX 命令将被保留并原封不动地传递给 LaTeX 和 ConTeXt 编写器。因此，例如，你可以使用 LaTeX 来包含 BibTeX 引用：

```
This result was proved in \cite{jones.1967}.
```

请注意，在 LaTeX 环境中，例如

```latex
\begin{tabular}{|l|l|}\hline
Age & Frequency \\ \hline
18--25  & 15 \\
26--35  & 33 \\
36--45  & 22 \\ \hline
\end{tabular}
```

开始和结束标签之间的内容将被解释为原始 LaTeX，而不是 Markdown。

有关在 Markdown 文档中包含原始 TeX 的更明确和灵活的方法，请参阅 [`raw_attribute` 扩展](https://pandoc.org/MANUAL.html?pandocs-markdown#extension-raw_attribute)。

在除 Markdown、LaTeX、Emacs Org 模式和 ConTeXt 之外的输出格式中，内联 LaTeX 会被忽略。

### 通用原始属性

**Extension: `raw_attribute`**

具有特殊属性的内联 span 和围栏代码块将被解析为具有指定格式的原始内容。例如，以下生成一个原始的 roff `ms` 块：

````
```{=ms}
.MYMACRO
blah blah
```
````

以下将生成一个原始的 html 内联元素：

```html
This is `<a>html</a>`{=html}
```

这对于将原始 xml 插入 docx 文档很有用，例如分页符：

````xml
```{=openxml}
<w:p>
  <w:r>
    <w:br w:type="page"/>
  </w:r>
</w:p>
```
````

格式名称应与目标格式名称相匹配（请参阅上面的 [`-t/--to`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--to) 以获取列表，或使用 `pandoc --list-output-formats`）。将 `openxml` 用于 `docx` 输出，将 `opendocument` 用于 `odt` 输出，将 `html5` 用于 `epub3` 输出，将 `html4` 用于 `epub2` 输出，将 `latex`、`beamer`、`ms` 或 `html5` 用于 `pdf` 输出（取决于你对 [`--pdf-engine`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--pdf-engine) 使用的是什么）。

此扩展假定相关类型的内联代码或围栏代码块已启用。因此，要使用带有反引号代码块的原始属性，必须启用 `backtick_code_blocks`。

原始属性不能与常规属性结合使用。

## LaTeX 宏

**Extension: `latex_macros`**

启用此扩展后，pandoc 将解析 LaTeX 宏定义并将生成的宏应用于所有 LaTeX 数学和原始 LaTeX。例如，以下将适用于所有输出格式，而不仅仅是 LaTeX：

```latex
\newcommand{\tuple}[1]{\langle #1 \rangle}

$\tuple{a, b, c}$
```

请注意，如果 LaTeX 宏出现在标有 [`raw_attribute` 扩展](https://pandoc.org/MANUAL.html?pandocs-markdown#extension-raw_attribute)的原始 span 或块内，则 pandoc 不会应用它们。

当 `latex_macros` 被禁用时，原始 LaTeX 和数学将不会应用宏。当您以 LaTeX 或 PDF 为目标时，这通常是更好的方法。

仅当未启用 `latex_macros` 时，LaTeX 中的宏定义才会作为原始 LaTeX 传递。无论是否启用 `latex_macros`，Markdown 源码（或其他允许 `raw_tex` 的格式）中的宏定义都将被传递。

## 链接

Markdown 允许以多种方式指定链接。

### 自动链接

如果您将 URL 或电子邮件地址括在尖括号中，它将成为一个链接：

```
<http://example.com>
<exmaple@example.com>
```

### 内联链接

内联链接由方括号中的链接文本和后跟圆括号中的 URL 组成。（此外，你还可以在 URL 后面附上一个用引号括起来的链接标题。）

```
这是一句[演示](http://example.com "链接标题")文本。
```

括号内的部分和括号内的部分之间不能有空格。链接文本可以包含格式（例如强调），但链接标题不可以。

内联链接中的电子邮件地址不会被自动检测到，因此它们必须以 `mailto` 为前缀：

```
[请写信给我](mailto:exmaple@example.com)
```

### 引用链接

**显式**引用链接由两部分组成，分别是：链接本身和链接定义；后者可能出现在文档的其他地方（链接之前或之后）。

链接由第一个方括号中的链接文本和第二个方括号中的标签组成（除非启用了 `spaced_reference_links` 扩展，否则两个方括号之间不能有空格）。

链接定义由被括号围起来的标签、冒号和空格，然后是 URL，以及可能存在的（在空格之后）用引号或括号围起来的链接标题所组成。标签不能作为引用（citation）进行解析（假设启用了  `citations` 扩展）：引用（citations）优先于链接标签（Reference links' labels）。

以下是一份样例：

```
[my label 1]: /foo/bar.html  "My title, optional"
[my label 2]: /foo
[my label 3]: https://fsf.org (The Free Software Foundation)
[my label 4]: /bar#special  'A title in single quotes'
```

URL 可以选择用尖括号括起来：

```
[my label 5]: <http://foo.bar.baz>
```

标题可能在下一行：

```
[my label 3]: https://fsf.org
  "The Free Software Foundation"
```

请注意，链接标签不区分大小写。所以，以下的用法也是有效的：

```
Here is [my link][FOO]

[Foo]: /bar/baz
```

在**隐式**引用链接中，第二对括号为空：

```
See [my website][].

[my website]: http://foo.bar.baz
```

注意：在 `Markdown.pl` 和大多数其他 Markdown 实现中，引用链接定义不能出现在嵌套结构中，例如列表项或块引号。Pandoc 取消了这个看似武断的限制。所以以下内容在 pandoc 中可以正常工作，但在大多数其他实现中则不行：

```
> My block [quote].
>
> [quote]: /foo
```

**Extension: `shortcut_reference_links`**

在快捷引用链接中，第二对括号可以完全省略：

```
See [my website].

[my website]: http://foo.bar.baz
```

### 内部链接

要链接到同一文档的另一部分，请使用自动生成的标识符（请参阅[标题标识符](#identifiers)）。例如：

```
See the [Introduction](#introduction).
```

或

```
See the [Introduction].

[Introduction]: #introduction
```

目前支持 HTML 格式（包括 HTML 幻灯片放映和 EPUB）、LaTeX 和 ConTeXt 的内部链接。

## 图像

一个链接前面紧跟着一个 `!` 将被视为图像。链接文本将用作图像的替代文本：

```
![image1](example.png "image tile")

![image2]

[image2]: example.jpg
```

**Extension: `implicit_figures`**

带有非空替代文本的图像在段落中单独出现，将呈现为带有标题的图形。图像的替代文本将用作标题。

```
![这是标题](/url/of/image.png)
```

如何呈现取决于输出格式。某些输出格式（例如 RTF）尚不支持图形。在这些格式中，您只会在段落中单独获得一张没有标题的图片。

如果您只想要一个常规的内联图像，只需确保它不是段落中的唯一内容。一种方法是在图像后插入一个不间断的空格。

请注意，在 reveal.js 幻灯片放映中，段落中具有 `r-stretch` 类的图像将填满屏幕，标题和图形标签将被省略。

**Extension: `link_attributes`**

可以在链接和图像上设置属性：

```
An inline ![image](foo.jpg){#id .class width=30 height=20px}
and a reference ![image][ref] with attributes.

[ref]: foo.jpg "optional title" {#id .class key=val key2="val 2"}
```

（当仅使用 `#id` 和 `.class` 时，此语法与 [PHP Markdown Extra](https://michelf.ca/projects/php-markdown/extra/) 兼容。）

对于 HTML 和 EPUB，除了 `width` 和 `height`（但包括 `srcset` 和 `sizes`）之外的所有已知 HTML5 属性都按原样传递。未知属性作为自定义属性传递，并带有数据前缀。其他输出器忽略了其输出格式未特别支持的属性。

pandoc 会对图像的 `width` 和 `height` 属性进行特殊处理。在没有确定单位时，pandoc 假定单位为 pixel。但是你可以使用以下任何单位标识符：px、cm、mm、in、inch 和 %。数字和单位之间不能有任何空格。例如：

```
![](file.jpg){ width=50% }
```

- 尺寸可以转换为与输出格式兼容的形式（例如，在将 HTML 转换为 LaTeX 时，以像素为单位的尺寸将转换为英寸）。像素和物理测量值之间的转换受 [`--dpi`](https://pandoc.org/MANUAL.html?pandocs-markdown#option--dpi) 选项的影响（默认情况下，假定为 96dpi，除非图像本身包含 dpi 信息）。
- % 单位一般是相对于一些可用空间的。例如，上面的示例将呈现为以下内容。  
    - HTML: `<img href="file.jpg" style="width: 50%;" />`  
    - LaTeX:  
    `\includegraphics[width=0.5\textwidth,height=\textheight]{file.jpg}` （如果你使用的是自定义模板，则需要像在默认模板中一样配置 `graphicx`。）  
    - ConTeXt：  
    `\externalfigure[file.jpg][width=0.5\textwidth]`  
- 某些输出格式具有类（[ConTeXt](https://wiki.contextgarden.net/Using_Graphics#Multiple_Image_Settings)）或唯一标识符 (LaTeX `\caption`) 或两者兼有（HTML）的概念。
- 当未指定 `width` 或 `height` 属性时，pandoc 会查看图像分辨率和图像文件中嵌入的 dpi 元数据。

## Div 和 Span

使用 `native_divs` 和 `native_spans` 扩展（见[上文](https://pandoc.org/MANUAL.html?pandocs-markdown#extension-native_divs)），HTML 语法可以用作 markdown 的一部分，以在 pandoc AST（与原始 HTML 相反）中创建原生 `Div` 和 `Span` 元素。但是，还有更好的语法可用：

**Extension: `fenced_divs`**

pandoc 允许对原生 `Div` 块使用特殊的围栏语法。Div 以包含至少三个连续冒号和一些属性的栅栏开始。属性后面可以有选择地跟另一串连续的冒号。属性语法与围栏代码块中的完全相同（请参阅[扩展：`fenced_code_attributes`](#fenced_code_blocks)）。与围栏代码块一样，可以使用大括号中的属性或单个未加括号的单词，这将被视为类名（class name）。Div 以包含至少三个连续冒号的字符串的另一行结尾。被围起来的 Div 应该用空行与前后文分开。

```
::::: {#special .sidebar}
Here is a paragraph.

And another.
:::::
```

围栏 div 可以嵌套。开始围栏（Opening fences）之所以有区别，是因为它们**必须**具有属性：

```
::: Warning ::::::
This is a warning.

::: Danger
This is a warning within a warning.
:::
::::::::::::::::::
```

没有属性的栅栏总是结束围栏（closing fences）。与围栏代码块不同，结束围栏中的冒号数量不需要与开始围栏中的冒号数量相匹配。但是，使用不同长度的栅栏将嵌套的 div 与其父级区分开来有助于提高源码的可读性。

**Extension: `bracketed_spans`**

一个带括号的内联序列，就像人们用来开始一个链接一样，如果它后面紧跟着属性，将被视为一个带有属性的 `Span`：

```
[This is *some text*]{.class key="val"}
```

## 脚注

**Extension: `footnotes`**

Pandoc 的 Markdown 允许使用脚注，语法如下：

```
这是一个脚注引用[^1]，这是另一个[^longnote]。

[^1]: 这是脚注文本。

[^longnote]: 这是另一个由多个块组成的脚注

    随后的段落缩进以表明它们属于前一个脚注。

        { 缩进四个空格的源码 }

    可以缩进整个段落，也可以只缩进第一行。这样，多段落脚注就像多段落列表项一样工作。

这一段文本不属于脚注的一部分，因为它没有缩进。
```

脚注引用中的标识符不能包含空格、制表符或换行符。这些标识符仅用于将脚注引用与注释本身相关联；在输出中，脚注将按顺序编号。

脚注本身不需要放在文件的末尾。它们可以出现在除其他块元素（列表、块引号、表格等）之外的任何地方。每个脚注应与周围的内容（包括其他脚注）用空行分隔。

**Extension: `inline_notes`**

内联脚注也是允许的（尽管与常规注释不同，它们不能包含多个段落）。语法如下：

```
这是一段演示文本[^内联脚注并不复杂，你只需要直接在括号内标明注释文本即可。]
```

内联脚注和常规脚注可以自由混合使用。

## 引用语法


## 非默认扩展


## markdown 变体

除了 pandoc 的扩展 Markdown 之外，pandoc 还支持以下 Markdown 变体：

- `markdown_phpextra` (PHP Markdown Extra)
- `markdown_github` (deprecated GitHub-Flavored Markdown)
- `markdown_mmd` (MultiMarkdown)
- `markdown_strict` (Markdown.pl)
- `commonmark` (CommonMark)
- `gfm` (Github-Flavored Markdown)
- `commonmark_x` (CommonMark with many pandoc extensions)

要查看给定格式支持哪些扩展，哪些是默认启用的，你可以使用如下命令：

```
pandoc --list-extensions=FORMAT
```

将其中 `FORMAT` 替换为具体格式的名称即可。

请注意，`commonmark`、`gfm` 和 `commonmark_x` 的扩展列表是相对于默认 `commonmark` 定义的。例如，backtick_code_blocks 不会作为扩展出现，因为它默认启用且无法禁用。

----

## 术语表 {#terms}

!!! note "注意"

    术语将按照出现的先后顺序进行列举。

- 段落：paragraph
- 换行符：newline
- 硬换行：hard line break
- 标题：heading
- 内联格式：inline formatting
- 标题标识符：heading identifier
- 链接锚点：link anchor
- 引用，参考：reference
- 块引用：block quotation
- 逐字块：verbatim block
- 围栏代码块：fenced code block
- 缩进代码块：indented code block
- 竖线块：Line block
- 项目列表：bullet list
- 列表项符号：list marker
- 定义列表：definition list
- 编号示例列表：numbered example list
- 编号示例：numbered example
- 表格说明：tables caption
- 多行表格：multiline table
- 网格表：grid table
- 管道表格：pipe table
- 标题块：title block
- 文本高亮：highlight
- 强调：emphasis
- 上标：superscript
- 下标：subscript
- 逐字文本：verbatim
- 下划线：underline
- 小型大写字母：small caps
- 定界符：delimiter
- 宏：macro
- 链接定义：link definition
- 脚注：footnote



[^1]: 这条规则的目的是确保正常的段落以人们的首字母开头，比如

      `B. Russell was an English philosopher.`

      不被视为列表项。但这条规则不会阻止诸如

      `(C) 2007 Joe Smith`

      免于被解释为列表项。在这种情况下，可以使用反斜杠转义：

      `(C\) 2007 Joe Smith`

[^2]: 我受到了 [David Wheeler](https://justatheory.com/2009/02/modest-markdown-proposal/) 的建议的影响。

[^3]: 这个方案是 Michel Fortin 提出的，他在 [Markdown 讨论列表](http://six.pairlist.net/pipermail/markdown-discuss/2005-March/001097.html)中提出了这个方案。

[^4]: 此处应该指的是 pandoc 的组件之一。

[^5]: 此处指的是使用一行中的两个尾随空格来表示硬换行符。