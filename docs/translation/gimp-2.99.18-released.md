---
comments: true
tags:
  - 新闻
  - GIMP
---

# GIMP 2.99.18 已发布：3.0 版本发布前最后的开发预览

- 译文信息：
    - 源文：[GIMP 2.99.18 Released: The Last Development Preview Before 3.0!](https://www.gimp.org/news/2024/02/21/gimp-2-99-18-released/)
    - 作者：[Wilber](https://www.gimp.org/author/wilber.html)
    - 许可证：[CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
    - 日期：2024-02-21
    - 译者：暮光的白杨
    - 补档翻译日期：2024-03-28

----

终于，我们为你带来了 GIMP 3 之前的最终开发版本！虽然 2.99.18 版本的发布比[原定计划]晚了一些，但我们非常高兴能与大家分享许多新功能和改进。

[原定计划]: https://gitlab.gnome.org/GNOME/gimp/-/issues/10373#timeline

⚠️ ☢️

我们提醒大家，**开发版本**意味着这是一个展示工作进展的版本，同时也为社区提供了早期发现问题和报告问题的机会。换句话说，这是一个不稳定的版本，我们不建议在生产中使用它。当你想通过[报告 bug] 来帮助改进 GIMP 时请使用此开发版本。

[报告 bug]: https://gitlab.gnome.org/GNOME/gimp/-/issues

由于 “space invasion[^space]” 项目，2.99.18 版本可能是 2.99 系列中最不稳定的版本之一。 这是预料之中的，也是正常的。

[^space]: *"[Space Invasion]" 是一个长期项目的代号，始于 GIMP 2.10 的开发，即早在 2012 年。其理念是让 GIMP 不仅仅是一个 sRGB 编辑器。起初，我们专注于 [anyRGB]，但如今我们更加关注对 [anySpace] 的支持。我们也希望能使用 [CMYK] 后端编辑图像（现在，我们可以进行 CMYK 🗘 RGB 转换，这很好，但还不够），还允许使用随机通道（如专色通道），当然，为什么不能使用 [CIELAB] 图像或其他有用的东西呢？*  
  详见：<https://developer.gimp.org/core/roadmap/#space-invasion>

[Space Invasion]: https://developer.gimp.org/core/roadmap/#space-invasion
[anySpace]: https://en.wikipedia.org/wiki/Color_space
[anyRGB]: https://en.wikipedia.org/wiki/RGB_color_spaces
[CMYK]: https://en.wikipedia.org/wiki/CMYK_color_model
[CIELAB]: https://en.wikipedia.org/wiki/CIELAB_color_space

⚠️ ☢️

*本新闻发布列出了最显著、最明显的更改。我们不会在此列出次要的错误修复或较小的改进。要获得更完整的变更列表，请参阅[新闻]文件或查看[提交历史]。*

[新闻]: https://gitlab.gnome.org/GNOME/gimp/-/blob/1f85924e3cbd7a3e24f3bcb23cd854433d9b5271/NEWS#L9
[提交历史]: https://gitlab.gnome.org/GNOME/gimp/-/commits/master

---

## (颜色) Space Invasion

我们一直在 [Space Invasion] 项目上非常努力地工作，大家[可能还记得]，这是我们使 GIMP 在颜色方面更加**准确**的项目的代号。

[可能还记得]: https://www.gimp.org/news/2018/08/19/gimp-2-10-6-released/#prepare-for-the-space-invasion

最近，我们一直在移植旧的内部颜色结构（`GimpRGB`、`GimpCMYK`、`GimpHSV`……），我们用它们将颜色信息传递给 [`GeglColor`](https://developer.gimp.org/api/gegl/class.Color.html)。这个通用对象可以包含任何颜色数据，无论我们的像素编码引擎 [babl] 支持的[颜色模型]、精度或[空间][anySpace]如何。
 
[颜色模型]: https://en.wikipedia.org/wiki/Color_model
[babl]: https://gegl.org/babl/

对于颜色准确性来说，我们现在只有在需要时才会进行色彩转换（最后一秒转换），因此不会丢失本可以避免的信息。例如，假设你从图像中提取颜色：如果我们要将其转换为中间格式，然后再用于第二张图像（可能是也可能不是另一种颜色格式），那么我们将进行两次转换。这意味着精度损失的可能性更大。如果输入和输出格式相同（即根本不应该发生转换），那么问题就更加明显。当我们拥有核心 [CMYK] 后端时，这个问题会更加严重（我们确实希望避免使用 CMYK 进行中间格式的往返转换，因为 CMYK 与大多数其他色彩模型之间都不存在双向转换，即使在无约束工作和忽略精度问题的情况下也是如此）。

我们也在慢慢将存储数据转移到这个通用颜色对象（`GeglColor`）中。这尤其意味着调色板将可以包含 CMYK 颜色、[CIELAB] 颜色或任何其他支持的模型（而不仅仅是转换为无约束 sRGB（Unbounded sRGB[^srgb]）后的这些颜色）。

[^srgb]: [Limitations of unbounded sRGB as a universal color space for image editing](https://ninedegreesbelow.com/photography/unbounded-srgb-as-universal-working-space.html)

这对代码维护的一个好处是，由于结构中包含了数据及其"含义（meaning）"，因此在代码库中处理颜色转换就容易多了。与我们必须将这两种信息作为单独数据进行跟踪相比，它使颜色处理的错误率大大降低。

最后，我们正努力在界面的各个部分显示相关的[色彩空间][anySpace]信息，例如在显示或选择 RGB、CMYK、HSL 或 HSV 数据时。色彩模型中这些没有关联色彩空间的值几乎毫无意义。在界面上显示 RGB 数值而不提供其他信息，是过去的残余，因为过去主要指的是 `sRGB`。在现代图形工作中，这显然不再准确，界面应该清楚地表明这一点。

下面的视频展示了部分界面工作，例如 RGB、HSV 或 CMYK 模型始终显示值所处的色彩空间（通常意味着 ICC 配置文件的名称）。在颜色选择器工具、颜色样本、可停靠的前景/背景颜色、"更改前景/背景颜色" 对话框以及其他更多地方都是如此。