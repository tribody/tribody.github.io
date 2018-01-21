---
title: 使用sublime+texlive搭建中文tex集成环境
date: 2017-11-21 11:45:00
tags: Tex
categories: 技术
---

## Tex简介
TeX是一种排版程序，最初由D.E.Knuth编写，在1982年之后就没有多大变化了。

TeX本身也是一门语言，因此基于TeX编写了很多的“TeX宏包”，它实际上是扩展了TeX的功能，这其中比较出名的就是LaTeX。这些宏包被称之为“format”。

目前LaTeX的版本叫做LaTeXe。

现在我们常用的MiTeX、teTeX等等称之为TeX的发行版，它是将Tex和许许多多的程序一起打包发布。Tex
和MiTeX、teTeX之间的关系，就好像linux与Debian、Ubuntu之间的关系。
<!-- more -->
中文常用的Tex发行版有MiTeX、TeXLive和MacTeX，除此之外还有CTeX项目。

废话不多说了，

本人的安装环境以及使用软件版本如下：

- windows 10专业版 1607版本
- texlive2016
- sublimetext 3

## 环境搭建
首先需要安装TeXLive2016，至于为什么用这个版本而不用2017（睡不想用最新版= =），因为我在win10上安装2017成功后，发现编译不了tex模板（硕士论文模板），输出错误应该是qt版本太低。可能需要升级Qt，相关解答在这里：

- [stackexchange](https://tex.stackexchange.com/questions/85792/miktex-x64-on-windows-8-qt-error)
- [Qt Forum](https://forum.qt.io/topic/64887/how-to-get-rid-of-the-qt-untested-windows-version-10-0-detected-warning) 

基本上说的都是替换texlive中的qt版本，这个未来官方可能会修复。

除此之外还有点儿小错误，太懒了，不想一个个去弄，急着用，以后可能会弄一下，于是就卸载掉了，重新安装模板推荐的texlive2016。注意安装texlive2016之前，需要在环境变量中添加`%SystemRoot%\system32`（因为win10中默认没有该变量路径，而texlive2016在安装的时候会搜索该路径）。

### 安装TeXLive2016
我是下载的texlive2016的ISO镜像，加载镜像安装的时候，可选自定义模式，如下图：

![texlive2016](/img/texlive2016.png)

在装好texlive2016之后，需要安装gb7714-2015字符集库。首先更新一下texlive manager，然后在texlive manager中安装该字符集（最好是选择国内镜像源，会快很多）。具体如下图：

![tlmgr安装方式](/img/tlmgr.png)

### 安装sublime text3
接下来安装sublime text3（2也行，但显然3有更炫酷的效果），我非常喜欢sublime所以才有这么一个教程。安装好sublime后，先去装个package control（sublime的插件管理器），然后使用package control安装一个叫做LaTeXTools的插件（按组合键`Ctrl + Shift + P`，然后输入install，选择Package Control: install package，搜索LaTeXTools，按Enter键即可）。

### 配置LaTeXTools插件
安装好后，首先下载[sumatraPDF](http://www.sumatrapdfreader.org/)，至于为什么用这个来预览Pdf，是因为它支持反向查找，这是一个非常神奇的功能，里可以随时找到Pdf某一个位置在tex文本中对应的位置，操作足够简单，双击即可。按账号sumatraPDF之后，需要在环境变量PATH中添加执行路径。然后在打开sumatraPDF，设置>>选项，设置反向搜索路径`"D:\Program Files\Sublime Text 3\sublime_text.exe" "%f:%l"`（或者直接在命令行中输入：`sumatrapdf.exe -inverse-search "\"C:\Program Files\Sublime Text 3\sublime_text.exe\" \"%f:%l\""
`），具体路径以sumlime安装目录为准。这样，当你编译完tex文件后，就能绑定两者了！

接下来配置LaTeXTools的图片和公式预览功能，首先texlive2016没有自带imagesticks，因此无法预览图片，但是有自带ghostscript（预览公式用）。需要我们自己下载安装[ImageMagick](http://www.imagemagick.org/)（预览图片用）。然后，我们需要在sumlime的preference>>package settings>>latex tools>>setting-user中找到windows对应位置的配置项，在texpath路径中分别添加ghostscript和imagemagick的可执行文件路径以及系统环境变量PATH。例如`"texpath" : "D:\\texlive\\2016\\bin\\win32;D:\\texlive\\2016\\tlpkg\\tlgs\\bin;D:\\Program Files\\ImageMagick-7.0.7-Q16;$PATH"`。到此，基本配置完毕，你就可以安心使用sumlime写你的论文了！

除此之外，还有很多可配置项，都在`setting-user`中可以配置。

## 使用
LaTexTools比较使用的功能有三个：

- 公式预览：点击你输入的公式，停留一段时间后，会将渲染结果显示在公式下方。
- 图片预览：将鼠标停留在包含图片的命令上，会弹出图片预览效果。
- 与SumatraPDF的配合：在SumatraPDF中双击相应的内容，回跳到sublime text3中的对应位置。

![fomulation](/img/formulation.png)

以上功能是默认的，当然可以配置具体的反馈机制。另外，使用`Ctrl + B`编译文档（具体的builid方式也可以自定义）。

另外，LaTeX官方有较详细的文档可供阅读[LatexTools](https://latextools.readthedocs.io/en/latest/)。

## 相关问题

### XeLatex在windows上编译太慢怎么办？

如题，最近发现sublime text + texlive2016编译tex巨慢无比。百度了一下，XeLatex在字体读入的地方会很慢。一般情况下如果找到字体编译完成，字体缓存也会得到刷新，后续编译速度相对就会很快。但是Windows下的texlive 2016实现有问题，自动刷新字体缓存也会有问题，此时需要手动刷新。手动刷新方式为`fc-cache`，另外，可以加上`-f`（强制刷新）和`-v`（显示详细信息）选项。

### warning很烦，如何关闭？

设置警告级别为只报错误，具体在Preference>>Package Settings>>LatexTools>>Settings-User中，找到关键词`show_error_phantoms`设为`errors`即可。

### 图片预览不了？

两种可能性，

- ImageMagick未配置到环境变量中；
- 引用的图片路径不对，在引用图片路径时，是相对于当前tex文件的路径。

## 参考文献：

- [LaTeXTools官方文档](https://latextools.readthedocs.io/en/latest/)
- [TeX — Beauty and Fun](http://www.ctex.org/documents/shredder/tex_frame.html)
- [XeLaTeX 编译时间太长是什么原因？](https://www.zhihu.com/question/53981204)