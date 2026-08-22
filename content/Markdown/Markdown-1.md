---
title: Markdown的基本语法(1)
date: 2024-10-01 18:26
updated: 2024-10-03 17:51
categories: 学习笔记
cover: 
series: Markdown的语法
tags:
- Markdown
- HTMl
---

以下内容参考 [Markdown官方教程](https://markdown.com.cn/)
仅供学习使用🎃🎃🎃

# 基本语法
---
Markdown是一种轻量级标记语言，排版语法简洁，让人们更多地关注内容本身而非排版。它使用易读易写的纯文本格式编写文档，可与HTML混编，可导出 HTML、PDF 以及本身的 .md 格式的文件。因简洁、高效、易读、易写，Markdown被大量使用，如Github、Wikipedia、简书等。

在线体验一下 Markdown在线编辑器。

千万不要被「标记」、「语言」吓到，Markdown的语法十分简单，常用的标记符号不超过十个，用于日常写作记录绰绰有余，不到半小时就能完全掌握。

就是这十个不到的标记符号，却能让人<strong>优雅地沉浸式记录，专注内容而不是纠结排版</strong>，达到「心中无尘，码字入神」的境界。

# 标题语法
---
要创建标题，请在单词或短语前面添加井号 (`#`) 。`#` 的数量代表了标题的级别。例如，添加三个 `#` 表示创建一个三级标题 (`<h3>`) (例如：`### My Header`)。

| Markdown语法             |   HTML                              |    预览效果  |
| :-----------:             | :---------:                           | :----:      |
| `# Heading level 1 `    | `<h1>Heading level 1</h1>`           |  <h1>Heading level 1</h1>     |
| `## Heading level 2`    | `<h2>Heading level 2</h2>`           |   <h2>Heading level 2</h2>      |
| `### Heading level 3`   |   `<h3>Heading level 3</h3>`         |  <h3>Heading level 3</h3>      |
| `#### Heading level 4`   |   `<h4>Heading level 4</h4>`         |  <h4>Heading level 4</h4>      |
| `##### Heading level 5`   |   `<h5>Heading level 5</h5>`         |  <h5>Heading level 5</h5>      |
| `###### Heading level 6`   |   `<h6>Heading level 6</h6>`         |  <h6>Heading level 6</h6>      |

## 可选语法
还可以在文本下方添加任意数量的 == 号来标识一级标题，或者 -- 号来标识二级标题。

| Markdown语法                                    |   HTML                              |    预览效果  |
| -----------                                    | :---------:                           | :----:      |
| `Heading level 1  `<br>`===============`	  | `<h1>Heading level 1</h1>`           |  <h1>Heading level 1</h1>     |
| `Heading level 2`<br>`---------------`    | `<h2>Heading level 2</h2>`           |   <h2>Heading level 2</h2>      |

## 最佳实践
不同的 Markdown 应用程序处理 `#` 和标题之间的空格方式并不一致。为了兼容考虑，请用一个空格在 `#` 和标题之间进行分隔。

|  ✅&nbsp; Do this |  ❌&nbsp; Don't do this  |  
| :---------------:          |     :---------------:              |
| ` # Here's a Heading`      |     `#Here's a Heading`            |

# 段落语法
---
要创建段落，请使用空白行将一行或多行文本进行分隔。

| Markdown语法             |   HTML                              |    预览效果  |
| :-----------             | :---------                          | :----      |
| `I really like using Markdown.`<br><br>`I think I'll use it to format all of my documents from now on.`  | `<p>I really like using Markdown.</p>`<br><br>`<p>I think I'll use it to format all of my documents from now on.</p>`    | I really like using Markdown.<br><br>I think I'll use it to format all of my documents from now on.    |

## 段落（Paragraph）用法的最佳实践
不要用空格（spaces）或制表符（ tabs）缩进段落。

|  ✅&nbsp; Do this |  ❌&nbsp; Don't do this  |  
| :---------------         |     :---------------              |
| `Don't put tabs or spaces in front of your paragraphs.`<br><br>`Keep lines left-aligned like this.`     |     <code>&nbsp;&nbsp;&nbsp;&nbsp;This can result in unexpected formatting problems.<br><br>&nbsp;&nbsp;Don't add tabs or spaces in front of paragraphs.</code>          |

# 换行语法
---
在一行的末尾添加两个或多个空格，然后按回车键,即可创建一个换行(`<br>`)。

| Markdown语法	|HTML    |	预览效果|  
| :------     | :---- |  :----  |
|  <code>This is the first line.&nbsp;<br>And this is the second line.</code> |  `<p>This is the first line.<br>And this is the second line.</p>`      |      <p>This is the first line.<br>And this is the second line.</p>    |

## 换行（Line Break）用法的最佳实践
几乎每个 Markdown 应用程序都支持两个或多个空格进行换行，称为 `结尾空格（trailing whitespace)` 的方式，但这是有争议的，因为很难在编辑器中直接看到空格，并且很多人在每个句子后面都会有意或无意地添加两个空格。由于这个原因，你可能要使用除结尾空格以外的其它方式来换行。幸运的是，几乎每个 Markdown 应用程序都支持另一种换行方式：HTML 的 `<br>` 标签。

为了兼容性，请在行尾添加“结尾空格”或 HTML 的 `<br>` 标签来实现换行。

还有两种其他方式我并不推荐使用。CommonMark 和其它几种轻量级标记语言支持在行尾添加反斜杠 (`\`) 的方式实现换行，但是并非所有 Markdown 应用程序都支持此种方式，因此从兼容性的角度来看，不推荐使用。并且至少有两种轻量级标记语言支持无须在行尾添加任何内容，只须键入回车键（`return`）即可实现换行。

|  ✅&nbsp; Do this |  ❌&nbsp; Don't do this  |  
| :---------------          |     :---------------              |
|    <code>First line with two spaces after.&nbsp;</code><br>`And the next line.`<br><br>`First line with the HTML tag after.<br>`<br>`And the next line.`<br>   |     `First line with a backslash after.\`<br>`And the next line.`<br><br>`First line with nothing after.`<br>`And the next line.`     |

# 强调语法
---
通过将文本设置为粗体或斜体来强调其重要性。

## 粗体（Bold）
要加粗文本，请在单词或短语的前后各添加两个星号（asterisks）或下划线（underscores）。如需加粗一个单词或短语的中间部分用以表示强调的话，请在要加粗部分的两侧各添加两个星号（asterisks）。

| Markdown语法 | HTML  |  预览效果 |
| ---------------          |     ---------------              | ------   |
|   `I just love **bold text**.`   |   `I just love <strong>bold text</strong>.`     |   I just love **bold text**.    |
|   `I just love __bold text__.`   |   `I just love <strong>bold text</strong>.`     |   I just love __bold text__.    |
|   `Love**is**bold`   |   `Love<strong>is</strong>bold`     |   Love**is**bold    |

### 粗体（Bold）用法最佳实践
Markdown 应用程序在如何处理单词或短语中间的下划线上并不一致。为兼容考虑，在单词或短语中间部分加粗的话，请使用星号（asterisks）。

|  ✅&nbsp; Do this |  ❌&nbsp; Don't do this  |  
| :---------------          |     :--------------- |
|    `Love**is**bold`   |    `Love__is__bold`    |

## 斜体（Italic）
要用斜体显示文本，请在单词或短语前后添加一个星号（asterisk）或下划线（underscore）。要斜体突出单词的中间部分，请在字母前后各添加一个星号，中间不要带空格。

| Markdown语法 | HTML  |  预览效果 |
| ---------------          |     ---------------              | ------   |
|   `Italicized text is the *cat's meow*.`  | `Italicized text is the <em>cat's meow</em>.`  | Italicized text is the *cat's meow*.    |
|   `Italicized text is the _cat's meow_.`   |   `Italicized text is the <em>cat's meow</em>.`     |  Italicized text is the _cat's meow_.    |
|   `A*cat*meow`   |   `A<em>cat</em>meow`     |   A*cat*meow    |

### 斜体（Italic）用法的最佳实践
要同时用粗体和斜体突出显示文本，请在单词或短语的前后各添加三个星号或下划线。要加粗并用斜体显示单词或短语的中间部分，请在要突出显示的部分前后各添加三个星号，中间不要带空格。

|  ✅&nbsp; Do this |  ❌&nbsp; Don't do this  |  
| :---------------          |     :--------------- |
|    `A*cat*meow`   |    `A_cat_meow`    |

## 粗体（Bold）和斜体（Italic）
要同时用粗体和斜体突出显示文本，请在单词或短语的前后各添加三个星号或下划线。要加粗并用斜体显示单词或短语的中间部分，请在要突出显示的部分前后各添加三个星号，中间不要带空格。

| Markdown语法 | HTML  |  预览效果 |
| ---------------          |     ---------------              | ------   |
|   `This text is ***really important***.`  | `This text is <strong><em>really important</em></strong>.`  | This text is ***really important***.    |
|   `This text is ___really important___.`   |   `This text is <strong><em>really important</em></strong>.`     |  This text is ___really important___.    |
|   `This is really***very***important text.`   |   `This is really<strong><em>very</em></strong>important text.`     |   This is really***very***important text.    |

### 粗体（Bold）和斜体（Italic）用法的最佳实践
Markdown 应用程序在处理单词或短语中间添加的下划线上并不一致。为了实现兼容性，请使用星号将单词或短语的中间部分加粗并以斜体显示，以示重要。

|  ✅&nbsp; Do this |  ❌&nbsp; Don't do this  |  
| :---------------          |     :--------------- |
|    `This is really***very***important text.`   |    `This is really___very___important text.`    |

# 引用语法
---
要创建块引用，请在段落前添加一个`>`符号。

<pre>> Dorothy followed her through many of the beautiful rooms in her castle.</pre>

渲染效果如下所示：

> Dorothy followed her through many of the beautiful rooms in her castle.

## 多个段落的块引用
块引用可以包含多个段落。为段落之间的空白行添加一个`>`符号。

<pre>> Dorothy followed her through many of the beautiful rooms in her castle.
>
> The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.</pre>

渲染效果如下所示：

> Dorothy followed her through many of the beautiful rooms in her castle.
>
> The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.

## 嵌套块引用
块引用可以嵌套。在要嵌套的段落前添加一个 `>>` 符号。

<pre>> Dorothy followed her through many of the beautiful rooms in her castle.
>
>> The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.</pre>

渲染效果如下所示：

> Dorothy followed her through many of the beautiful rooms in her castle.
>
>> The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.

## 带有其它元素的块引用
块引用可以包含其他 Markdown 格式的元素。并非所有元素都可以使用，你需要进行实验以查看哪些元素有效。

<pre>> #### The quarterly results look great!
>
> - Revenue was off the chart.
> - Profits were higher than ever.
>
>  *Everything* is going according to **plan**.</pre>

渲染效果如下所示：

> #### The quarterly results look great!
>
> - Revenue was off the chart.
> - Profits were higher than ever.
>
>  *Everything* is going according to **plan**.

# 列表语法
---
可以将多个条目组织成有序或无序列表。

## 有序列表
要创建有序列表，请在每个列表项前添加数字并紧跟一个英文句点。数字不必按数学顺序排列，但是列表应当以数字 1 起始。

| Markdown语法 | HTML  |  预览效果 |
| ---------------          |     ---------------              | ------   |
|   <code>1. First item<br>2. Second item<br>3. Third item<br>4. Fourth item</code>  | `<ol>`<br>`<li>First item</li>`<br>`<li>Second item</li>`<br>`<li>Third item</li>`<br>`<li>Fourth item</li>`<br>`</ol>`  | 1. First item<br>2. Second item<br>3. Third item<br>4. Fourth item    |
|   <code>1. First item<br>2. Second item<br>3. Third item<br>&nbsp;&nbsp;&nbsp;&nbsp;1. Indented item<br>&nbsp;&nbsp;&nbsp;&nbsp;2. Indented item<br>4. Fourth item</code>   |   `<ol>`<br>`<li>First item</li>`<br>`<li>Second item</li>`<br>`<li>Third item`<br>`<ol>`<br>`<li>Indented item</li>`<br>`<li>Indented item</li>`<br>`</ol>`<br>`</li>`<br>`<li>Fourth item</li>`<br>`</ol>`     |  <ol><li>First item</li><li>Second item</li><li>Third item<ol><li>Indented item</li><li>Indented item</li></ol><li>Fourth item</li>    |

## 代码块
代码块通常采用四个空格或一个制表符缩进。当它们被放在列表中时，请将它们缩进八个空格或两个制表符。

    1.  Open the file.
    2.  Find the following code block on line 21:

        <html>
          <head>
            <title>Test</title>
          </head>

    3.  Update the title to match the name of your website.

渲染效果如下：

1.  Open the file.
2.  Find the following code block on line 21:

        <html>
          <head>
            <title>Test</title>
          </head>

3.  Update the title to match the name of your website.

## 图片

    1.  Open the file containing the Linux mascot.
    2.  Marvel at its beauty.

    ![](/assets/r2.jpg)

    3.  Close the file.

渲染效果如下：

1.  Open the file containing the Linux mascot.
2.  Marvel at its beauty.

    ![](/assets/open.webp)

3.  Close the file.

# 链接语法
---

链接文本放在中括号内，链接地址放在后面的括号中，链接title可选。

超链接Markdown语法代码：`[超链接显示名](超链接地址 "超链接title")`

对应的HTML代码：`<a href="超链接地址" title="超链接title">超链接显示名</a>`

    这是一个链接 [Markdown语法](https://markdown.com.cn)。

渲染效果如下：

这是一个链接 [Markdown语法](https://markdown.com.cn)。

## 给链接增加 Title
链接title是当鼠标悬停在链接上时会出现的文字，这个title是可选的，它放在圆括号中链接地址后面，跟链接地址之间以空格分隔。

    这是一个链接 [Markdown语法](https://markdown.com.cn "最好的markdown教程")。

渲染效果如下：

这是一个链接 [Markdown语法](https://markdown.com.cn "最好的markdown教程")。

## 网址和Email地址
使用尖括号可以很方便地把URL或者email地址变成可点击的链接。

    <https://markdown.com.cn>
    <fake@example.com>

渲染效果如下：

<https://markdown.com.cn>
<fake@example.com>

## 带格式化的链接
强调链接, 在链接语法前后增加星号。 要将链接表示为代码，请在方括号中添加反引号。

    I love supporting the **[EFF](https://eff.org)**.
    This is the *[Markdown Guide](https://www.markdownguide.org)*.
    See the section on [`code`](#code).

渲染效果如下：

I love supporting the **[EFF](https://eff.org)**.
This is the *[Markdown Guide](https://www.markdownguide.org)*.
See the section on [`code`](#code).

# 图片语法
---
要添加图像，请使用感叹号 (`!`), 然后在方括号增加替代文本，图片链接放在圆括号里，括号里的链接后可以增加一个可选的图片标题文本。

插入图片Markdown语法代码：`![图片alt](图片链接 "图片title")`。

对应的HTML代码：`<img src="图片链接" alt="图片alt" title="图片title">`

如：

    ![这是图片](/assets/img/philly-magic-garden.jpg "Magic Gardens")

给图片增加链接，请将图像的Markdown 括在方括号中，然后将链接添加在圆括号中。

如:

    [![沙漠中的岩石图片](/assets/img/shiprock.jpg "Shiprock")](https://markdown.com.cn)

# HTML
---
Markdown支持原生HTML语法，譬如，你可以用 Html 写一个纵跨两行的表格：
代码：

    <table>
        <tr>
            <th rowspan="2">值班人员</th>
            <th>星期一</th>
            <th>星期二</th>
            <th>星期三</th>
        </tr>
        <tr>
            <td>李强</td>
            <td>张明</td>
            <td>王平</td>
        </tr>
    </table>

效果：

<table>
    <tr>
        <th rowspan="2">值班人员</th>
        <th>星期一</th>
        <th>星期二</th>
        <th>星期三</th>
    </tr>
    <tr>
        <td>李强</td>
        <td>张明</td>
        <td>王平</td>
    </tr>
</table>

也可以实现对字体格式的改变

代码：

    <font face="楷体" color=#00ffff size=5>改变文字格式</font>

效果：

<font face="楷体" color=#00ffff size=5>改变文字格式</font>

目前支持的 HTML 元素有：`<kbd>` `<b>` `<i>` `<em>` `<sup>` `<sub>` `<br>`等 

如：

    使用 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd> 重启电脑

效果：

使用 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd> 重启电脑

出于安全原因，并非所有 Markdown 应用程序都支持在 Markdown 文档中添加 HTML。如有疑问，请查看相应 Markdown 应用程序的手册。某些应用程序只支持 HTML 标签的子集。

对于 HTML 的块级元素 `<div>`、`<table>`、`<pre>` 和 `<p>`，请在其前后使用空行（blank lines）与其它内容进行分隔。尽量不要使用制表符（tabs）或空格（spaces）对 HTML 标签做缩进，否则将影响格式。

在 HTML 块级标签内不能使用 Markdown 语法。例如 `<p>italic and **bold**</p>` 将不起作用。

如有需要，请查阅其它笔记：
{% series Markdown的语法 %}