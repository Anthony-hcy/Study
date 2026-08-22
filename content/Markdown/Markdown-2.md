---
title: Markdown的基本语法(2)
date: 2024-10-04 18:26
updated: 2024-10-05 19:34
categories: 学习笔记
cover: 
series: Markdown的语法
mathjax: true
tags:
- Markdown
- HTMl
---

# 删除线
---

删除线的的使用，可以在要添加删除线的文字前后添加两个`~`
代码：

    ~~这是要被删除的文字~~

效果：

~~这是要被删除的文字~~

# 下划线
---
下划线的使用和html中类似，在需要添加下划线的文字首尾添加`<u>文本</u>`
代码：

    <u>这行文字已被添加下划线</u>

效果：

<u>这行文字已被添加下划线</u>

# 脚注
---
脚注是对文本的备注，我们时长在论文中看到脚注，在Markdown中的使用方法
代码：

    使用 Markdown[^1]可以效率的书写文档, 直接转换成 HTML[^2], 你可以使用 Typora[^T] 编辑器进行书写。
    [^1]:Markdown是一种纯文本标记语言
    [^2]:HyperText Markup Language 超文本标记语言
    [^T]:NEW WAY TO READ & WRITE MARKDOWN.

效果：

使用 Markdown[^1]可以效率的书写文档, 直接转换成 HTML[^2], 你可以使用 Typora[^T] 编辑器进行书写。
[^1]:Markdown 一种纯文本标记语言
[^2]:HyperText Markup Language 超文本标记语言
[^T]:NEW WAY TO READ & WRITE MARKDOWN.

# 制作待办事项
---
我们可以使用Markdown来制作一个待办事项，格式为、`-[]` 表示未完成；`-[x]`表示已完成
代码：

    - [ ] 支持以 PDF 格式导出文稿
    - [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
    - [x] 新增 Todo 列表功能
    - [x] 修复 LaTex 公式渲染问题
    - [x] 新增 LaTex 公式编号功能

效果：

- [ ] 支持以 PDF 格式导出文稿
- [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
- [x] 新增 Todo 列表功能
- [x] 修复 LaTex 公式渲染问题
- [x] 新增 LaTex 公式编号功能

# 书写公式
---
Markdown支持书写公式，例如书写一个质能守恒公式。

    $$...$$表示整行公式

代码：

    $$E=mc^2$$

效果：

$$E=mc^2$$

代码：

    $$
    \begin{Bmatrix}
    a & b \\
    c & d
    \end{Bmatrix}
    $$
    $$
    \begin{CD}
    A @>a>> B \\
    @VbVV @AAcA \\
    C @= D
    \end{CD}
    $$

效果：

$$
\begin{Bmatrix}
   a & b \\
   c & d
\end{Bmatrix}
$$
$$
\begin{CD}
   A @>a>> B \\
@VbVV @AAcA \\
   C @= D
\end{CD}
$$

# 流程图
---

{% tabs test %}

<!-- tab 例子 -->

代码：

```markdown
{% mermaid %}
pie
title Key elements in Product X
"Calcium" : 42.96
"Potassium" : 50.05
"Magnesium" : 10.01
"Iron" : 5
{% endmermaid %}
```

效果：

{% mermaid %}
pie
title Key elements in Product X
"Calcium" : 42.96
"Potassium" : 50.05
"Magnesium" : 10.01
"Iron" : 5
{% endmermaid %}


<!-- endtab -->

<!-- tab 横向流程图 -->

代码：

    mermaid
    graph LR
    A[方形] -->B(圆角)
        B --> C{条件a}
        C -->|a=1| D[结果1]
        C -->|a=2| E[结果2]

效果：

{% mermaid %}
graph LR
A[方形] -->B(圆角)
    B --> C{条件a}
    C -->|a=1| D[结果1]
    C -->|a=2| E[结果2]
{% endmermaid %}


<!-- endtab -->

<!-- tab 竖向流程图 -->

代码：

    mermaid
    graph TD
    A[方形] --> B(圆角)
        B --> C{条件a}
        C --> |a=1| D[结果1]
        C --> |a=2| E[结果2]

效果：

{% mermaid %}
graph TD
A[方形] --> B(圆角)
    B --> C{条件a}
    C --> |a=1| D[结果1]
    C --> |a=2| E[结果2]
{% endmermaid %}

<!-- endtab -->

<!-- tab 甘特图 -->

代码：

    mermaid
    %% 语法示例
            gantt
            dateFormat  YYYY-MM-DD
            title 软件开发甘特图
            section 设计
            需求                      :done,    des1, 2014-01-06,2014-01-08
            原型                      :active,  des2, 2014-01-09, 3d
            UI设计                     :         des3, after des2, 5d
        未来任务                     :         des4, after des3, 5d
            section 开发
            学习准备理解需求                      :crit, done, 2014-01-06,24h
            设计框架                             :crit, done, after des2, 2d
            开发                                 :crit, active, 3d
            未来任务                              :crit, 5d
            耍                                   :2d
            section 测试
            功能测试                              :active, a1, after des3, 3d
            压力测试                               :after a1  , 20h
            测试报告                               : 48h
    

效果：

{% mermaid %}
%% 语法示例
        gantt
        dateFormat  YYYY-MM-DD
        title 软件开发甘特图
        section 设计
        需求                      :done,    des1, 2014-01-06,2014-01-08
        原型                      :active,  des2, 2014-01-09, 3d
        UI设计                     :         des3, after des2, 5d
    未来任务                     :         des4, after des3, 5d
        section 开发
        学习准备理解需求                      :crit, done, 2014-01-06,24h
        设计框架                             :crit, done, after des2, 2d
        开发                                 :crit, active, 3d
        未来任务                              :crit, 5d
        耍                                   :2d
        section 测试
        功能测试                              :active, a1, after des3, 3d
        压力测试                               :after a1  , 20h
        测试报告                               : 48h
{% endmermaid %}



<!-- endtab -->

{% endtabs %}


如有需要，请查阅其它笔记：
{% series Markdown的语法 %}







