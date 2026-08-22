---
title: Markdown的高级语法(2)
date: 2024-10-11 18:50
updated: 2024-10-11 18:49
categories: 学习笔记
cover: 
series: Markdown的语法
tags:
- Markdown
- HTMl
- 标签外挂
---

{% note success modern %}
以下内容参考 [Tag Plugins Plus](https://akilar.top/posts/615e2dec/)
仅供学习使用🎃🎃🎃
{% endnote %}


# 行内文本样式 text

{% hideToggle Text %}

{% tabs test1 %}

<!-- tab 标签语法 -->

```markdown
{% u 文本内容 %}
{% emp 文本内容 %}
{% wavy 文本内容 %}
{% del 文本内容 %}
{% kbd 文本内容 %}
{% psw 文本内容 %}
```

<!-- endtab -->

<!-- tab 示例源码 -->

```markdown
1. 带 {% u 下划线 %} 的文本
2. 带 {% emp 着重号 %} 的文本
3. 带 {% wavy 波浪线 %} 的文本
4. 带 {% del 删除线 %} 的文本
5. 键盘样式的文本 {% kbd command %} + {% kbd D %}
6. 密码样式的文本：{% psw 这里没有验证码 %}
```

<!-- endtab -->

<!-- tab 样式预览 -->

1. 带 {% u 下划线 %} 的文本
2. 带 {% emp 着重号 %} 的文本
3. 带 {% wavy 波浪线 %} 的文本
4. 带 {% del 删除线 %} 的文本
5. 键盘样式的文本 {% kbd command %} + {% kbd D %}
6. 密码样式的文本：{% psw 这里没有验证码 %}

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}


# 行内文本 span

{% hideToggle Span %}

{% tabs test1 %}

<!-- tab 标签语法 -->

```markdown
{% span 样式参数(参数以空格划分), 文本内容 %}
```

<!-- endtab -->

<!-- tab 配置参数 -->

1. 字体: logo, code
2. 颜色: red,yellow,green,cyan,blue,gray
3. 大小: small, h4, h3, h2, h1, large, huge, ultra
4. 对齐方向: left, center, right

<!-- endtab -->

<!-- tab 示例源码 -->

```markdown
- 彩色文字
在一段话中方便插入各种颜色的标签，包括：{% span red, 红色 %}、{% span yellow, 黄色 %}、{% span green, 绿色 %}、{% span cyan, 青色 %}、{% span blue, 蓝色 %}、{% span gray, 灰色 %}。
- 超大号文字
文档「开始」页面中的标题部分就是超大号文字。
{% span center logo large, Volantis %}
{% span center small, A Wonderful Theme for Hexo %}
```

<!-- endtab -->

<!-- tab 样式预览 -->

- 彩色文字
在一段话中方便插入各种颜色的标签，包括：{% span red, 红色 %}、{% span yellow, 黄色 %}、{% span green, 绿色 %}、{% span cyan, 青色 %}、{% span blue, 蓝色 %}、{% span gray, 灰色 %}。
- 超大号文字
文档「开始」页面中的标题部分就是超大号文字。
{% span center logo large, Volantis %}
{% span center small, A Wonderful Theme for Hexo %}

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}


# 段落文本 p

{% hideToggle P %}

{% tabs test1 %}

<!-- tab 标签语法 -->

```markdown
{% p 样式参数(参数以空格划分), 文本内容 %}
```

<!-- endtab -->

<!-- tab 配置参数 -->

1. 字体: logo, code
2. 颜色: red,yellow,green,cyan,blue,gray
3. 大小: small, h4, h3, h2, h1, large, huge, ultra
4. 对齐方向: left, center, right

<!-- endtab -->

<!-- tab 示例源码 -->

```markdown
- 彩色文字
在一段话中方便插入各种颜色的标签，包括：{% p red, 红色 %}、{% p yellow, 黄色 %}、{% p green, 绿色 %}、{% p cyan, 青色 %}、{% p blue, 蓝色 %}、{% p gray, 灰色 %}。
- 超大号文字
文档「开始」页面中的标题部分就是超大号文字。
{% p center logo large, Volantis %}
{% p center small, A Wonderful Theme for Hexo %}
```

<!-- endtab -->

<!-- tab 样式预览 -->

- 彩色文字
在一段话中方便插入各种颜色的标签，包括：{% p red, 红色 %}、{% p yellow, 黄色 %}、{% p green, 绿色 %}、{% p cyan, 青色 %}、{% p blue, 蓝色 %}、{% p gray, 灰色 %}。
- 超大号文字
文档「开始」页面中的标题部分就是超大号文字。
{% p center logo large, Volantis %}
{% p center small, A Wonderful Theme for Hexo %}

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}


# 复选列表 checkbox

{% hideToggle Checkbox %}

{% tabs test1 %}

<!-- tab 标签语法 -->

```markdown
{% checkbox 样式参数（可选）, 文本（支持简单md） %}
```

<!-- endtab -->

<!-- tab 配置参数 -->

1. 样式: plus, minus, times
2. 颜色: red,yellow,green,cyan,blue,gray
3. 选中状态: checked

<!-- endtab -->

<!-- tab 示例源码 -->

```markdown
{% checkbox 纯文本测试 %}
{% checkbox checked, 支持简单的 [markdown](https://guides.github.com/features/mastering-markdown/) 语法 %}
{% checkbox red, 支持自定义颜色 %}
{% checkbox green checked, 绿色 + 默认选中 %}
{% checkbox yellow checked, 黄色 + 默认选中 %}
{% checkbox cyan checked, 青色 + 默认选中 %}
{% checkbox blue checked, 蓝色 + 默认选中 %}
{% checkbox plus green checked, 增加 %}
{% checkbox minus yellow checked, 减少 %}
{% checkbox times red checked, 叉 %}
```

<!-- endtab -->

<!-- tab 样式预览 -->

{% checkbox 纯文本测试 %}
{% checkbox checked, 支持简单的 [markdown](https://guides.github.com/features/mastering-markdown/) 语法 %}
{% checkbox red, 支持自定义颜色 %}
{% checkbox green checked, 绿色 + 默认选中 %}
{% checkbox yellow checked, 黄色 + 默认选中 %}
{% checkbox cyan checked, 青色 + 默认选中 %}
{% checkbox blue checked, 蓝色 + 默认选中 %}
{% checkbox plus green checked, 增加 %}
{% checkbox minus yellow checked, 减少 %}
{% checkbox times red checked, 叉 %}

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}


# 单选列表 radio

{% hideToggle Radio %}

{% tabs test1 %}

<!-- tab 标签语法 -->

```markdown
{% radio 样式参数（可选）, 文本（支持简单md） %}
```

<!-- endtab -->

<!-- tab 配置参数 -->

1. 颜色: red,yellow,green,cyan,blue,gray
2. 选中状态: checked

<!-- endtab -->

<!-- tab 示例源码 -->

```markdown
{% radio 纯文本测试 %}
{% radio checked, 支持简单的 [markdown](https://guides.github.com/features/mastering-markdown/) 语法 %}
{% radio red, 支持自定义颜色 %}
{% radio green, 绿色 %}
{% radio yellow, 黄色 %}
{% radio cyan, 青色 %}
{% radio blue, 蓝色 %}
```

<!-- endtab -->

<!-- tab 样式预览 -->

{% radio 纯文本测试 %}
{% radio checked, 支持简单的 [markdown](https://guides.github.com/features/mastering-markdown/) 语法 %}
{% radio red, 支持自定义颜色 %}
{% radio green, 绿色 %}
{% radio yellow, 黄色 %}
{% radio cyan, 青色 %}
{% radio blue, 蓝色 %}

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}


# 链接卡片 link

{% hideToggle Link %}

{% tabs test1 %}

<!-- tab 标签语法 -->

```markdown
{% link 标题, 链接, 图片链接（可选） %}
```

<!-- endtab -->

<!-- tab 示例源码 -->

```markdown
{% link Markdown教程, https://anthony-hcy.github.io/2024/10/05/%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0-Markdown-3/, https://tuchuang.voooe.cn/images/2024/07/01/1.png %}
```

<!-- endtab -->

<!-- tab 样式预览 -->

{% link Markdown教程, https://anthony-hcy.github.io/2024/10/05/%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0-Markdown-3/, https://tuchuang.voooe.cn/images/2024/07/01/1.png %}

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}


# github卡片 ghcard

{% hideToggle Ghcard %}

{% tabs test1 %}

<!-- tab 标签语法 -->

```markdown
{% ghcard 用户名, 其它参数（可选） %}
{% ghcard 用户名/仓库, 其它参数（可选） %}
```

<!-- endtab -->

<!-- tab 配置参数 -->

更多参数可以参考：
 {% ghcard anuraghazra/github-readme-stats, theme=buefy %}
使用`,`分割各个参数。写法为：`参数名=参数值`
以下只写几个常用参数值。

|参数名	|取值	|释义|
| --- | --- | --- |
|hide	|stars,commits,prs,issues,contribs	|隐藏指定统计|
|count_private	|true	|将私人项目贡献添加到总提交计数中|
|show_icons	|true	|显示图标|
|theme	|请查阅[Available Themes](https://github.com/anuraghazra/github-readme-stats/blob/master/themes/README.md)	|主题|

<!-- endtab -->

<!-- tab 示例源码 -->

1. 用户信息卡片
```markdown
| {% ghcard xaoxuu %} | {% ghcard xaoxuu, theme=vue %} |
| -- | -- |
| {% ghcard xaoxuu, theme=buefy %} | {% ghcard xaoxuu, theme=solarized-light %} |
| {% ghcard xaoxuu, theme=onedark %} | {% ghcard xaoxuu, theme=solarized-dark %} |
| {% ghcard xaoxuu, theme=algolia %} | {% ghcard xaoxuu, theme=calm %} |
```
2. 仓库信息卡片
```markdown
| {% ghcard volantis-x/hexo-theme-volantis %} | {% ghcard volantis-x/hexo-theme-volantis, theme=vue %} |
| -- | -- |
| {% ghcard volantis-x/hexo-theme-volantis, theme=buefy %} | {% ghcard volantis-x/hexo-theme-volantis, theme=solarized-light %} |
| {% ghcard volantis-x/hexo-theme-volantis, theme=onedark %} | {% ghcard volantis-x/hexo-theme-volantis, theme=solarized-dark %} |
| {% ghcard volantis-x/hexo-theme-volantis, theme=algolia %} | {% ghcard volantis-x/hexo-theme-volantis, theme=calm %} |
```

<!-- endtab -->

<!-- tab 样式预览 -->

__用户信息卡片__

| {% ghcard xaoxuu %} | {% ghcard xaoxuu, theme=vue %} |
| -- | -- |
| {% ghcard xaoxuu, theme=buefy %} | {% ghcard xaoxuu, theme=solarized-light %} |
| {% ghcard xaoxuu, theme=onedark %} | {% ghcard xaoxuu, theme=solarized-dark %} |
| {% ghcard xaoxuu, theme=algolia %} | {% ghcard xaoxuu, theme=calm %} |

__仓库信息卡片__

| {% ghcard volantis-x/hexo-theme-volantis %} | {% ghcard volantis-x/hexo-theme-volantis, theme=vue %} |
| -- | -- |
| {% ghcard volantis-x/hexo-theme-volantis, theme=buefy %} | {% ghcard volantis-x/hexo-theme-volantis, theme=solarized-light %} |
| {% ghcard volantis-x/hexo-theme-volantis, theme=onedark %} | {% ghcard volantis-x/hexo-theme-volantis, theme=solarized-dark %} |
| {% ghcard volantis-x/hexo-theme-volantis, theme=algolia %} | {% ghcard volantis-x/hexo-theme-volantis, theme=calm %} |

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}




# github徽标 ghbdage

{% hideToggle Ghbdage %}

{% note blue 'fas fa-bullhorn' simple %}
关于ghbdage参数的更多具体用法可以参看教程：[添加github徽标](https://akilar.top/posts/e87ad7f8)
{% endnote %}

{% tabs test1 %}

<!-- tab 标签语法 -->

```markdown
{% bdage [right],[left],[logo]||[color],[link],[title]||[option] %}
```

<!-- endtab -->

<!-- tab 配置参数 -->

1. `left`：徽标左边的信息，必选参数。
2. `right`: 徽标右边的信息，必选参数，
3. `logo`：徽标图标，图标名称详见[simpleicons](https://simpleicons.org/)，可选参数。
4. `color`：徽标右边的颜色，可选参数。
5. `link`：指向的链接，可选参数。
6. `title`：徽标的额外信息，可选参数。主要用于优化SEO，但`object`标签不会像`a`标签一样在鼠标悬停显示`title`信息。
7. `option`：自定义参数，支持[shields.io](https://shields.io/)的全部API参数支持，具体参数可以参看上文中的拓展写法示例。形式为`name1=value2&name2=value2`。

<!-- endtab -->

<!-- tab 示例源码 -->

1. 基本参数,定义徽标左右文字和图标
```markdown
{% bdage Theme,Butterfly %}
{% bdage Frame,Hexo,hexo %}
```
2. 信息参数，定义徽标右侧内容背景色，指向链接
```markdown
{% bdage CDN,JsDelivr,jsDelivr||abcdef,https://metroui.org.ua/index.html,本站使用JsDelivr为静态资源提供CDN加速 %}
//如果是跨顺序省略可选参数，仍然需要写个逗号,用作分割
{% bdage Source,GitHub,GitHub||,https://github.com/ %}
```
3. 拓展参数，支持shields的API的全部参数内容
```markdown
{% bdage Hosted,Vercel,Vercel||brightgreen,https://vercel.com/,本站采用双线部署，默认线路托管于Vercel||style=social&logoWidth=20 %}
//如果是跨顺序省略可选参数组，仍然需要写双竖线||用作分割
{% bdage Hosted,Vercel,Vercel||||style=social&logoWidth=20&logoColor=violet %}
```

<!-- endtab -->

<!-- tab 样式预览 -->

__基本参数__

{% bdage Theme,Butterfly %}
{% bdage Frame,Hexo,hexo %}

__信息参数__

{% bdage CDN,JsDelivr,jsDelivr||abcdef,https://metroui.org.ua/index.html,本站使用JsDelivr为静态资源提供CDN加速 %}
{% bdage Source,GitHub,GitHub||,https://github.com/ %}

__拓展参数__

{% bdage Hosted,Vercel,Vercel||brightgreen,https://vercel.com/,本站采用双线部署，默认线路托管于Vercel||style=social&logoWidth=20 %}
{% bdage Hosted,Vercel,Vercel||||style=social&logoWidth=20&logoColor=violet %}

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}



# 网站卡片 sites

{% hideToggle Sites %}

{% tabs test1 %}

<!-- tab 标签语法 -->

```markdown
{% sitegroup %}
{% site 标题, url=链接, screenshot=截图链接, avatar=头像链接（可选）, description=描述（可选） %}
{% site 标题, url=链接, screenshot=截图链接, avatar=头像链接（可选）, description=描述（可选） %}
{% endsitegroup %}
```

<!-- endtab -->

<!-- tab 示例源码 -->

```markdown
{% sitegroup %}
{% site xaoxuu, url=https://xaoxuu.com, screenshot=https://i.loli.net/2020/08/21/VuSwWZ1xAeUHEBC.jpg, avatar=https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/avatar/avatar.png, description=简约风格 %}
{% site inkss, url=https://inkss.cn, screenshot=https://i.loli.net/2020/08/21/Vzbu3i8fXs6Nh5Y.jpg, avatar=https://cdn.jsdelivr.net/gh/inkss/common@master/static/web/avatar.jpg, description=这是一段关于这个网站的描述文字 %}
{% site MHuiG, url=https://blog.mhuig.top, screenshot=https://i.loli.net/2020/08/22/d24zpPlhLYWX6D1.png, avatar=https://cdn.jsdelivr.net/gh/MHuiG/imgbed@master/data/p.png, description=这是一段关于这个网站的描述文字 %}
{% site Colsrch, url=https://colsrch.top, screenshot=https://i.loli.net/2020/08/22/dFRWXm52OVu8qfK.png, avatar=https://cdn.jsdelivr.net/gh/Colsrch/images/Colsrch/avatar.jpg, description=这是一段关于这个网站的描述文字 %}
{% site Linhk1606, url=https://linhk1606.github.io, screenshot=https://i.loli.net/2020/08/21/3PmGLCKicnfow1x.png, avatar=https://i.loli.net/2020/02/09/PN7I5RJfFtA93r2.png, description=这是一段关于这个网站的描述文字 %}
{% endsitegroup %}
```

<!-- endtab -->

<!-- tab 样式预览 -->

{% sitegroup %}
{% site xaoxuu, url=https://xaoxuu.com, screenshot=https://i.loli.net/2020/08/21/VuSwWZ1xAeUHEBC.jpg, avatar=https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/avatar/avatar.png, description=简约风格 %}
{% site inkss, url=https://inkss.cn, screenshot=https://i.loli.net/2020/08/21/Vzbu3i8fXs6Nh5Y.jpg, avatar=https://cdn.jsdelivr.net/gh/inkss/common@master/static/web/avatar.jpg, description=这是一段关于这个网站的描述文字 %}
{% site MHuiG, url=https://blog.mhuig.top, screenshot=https://i.loli.net/2020/08/22/d24zpPlhLYWX6D1.png, avatar=https://cdn.jsdelivr.net/gh/MHuiG/imgbed@master/data/p.png, description=这是一段关于这个网站的描述文字 %}
{% site Colsrch, url=https://colsrch.top, screenshot=https://i.loli.net/2020/08/22/dFRWXm52OVu8qfK.png, avatar=https://cdn.jsdelivr.net/gh/Colsrch/images/Colsrch/avatar.jpg, description=这是一段关于这个网站的描述文字 %}
{% site Linhk1606, url=https://linhk1606.github.io, screenshot=https://i.loli.net/2020/08/21/3PmGLCKicnfow1x.png, avatar=https://i.loli.net/2020/02/09/PN7I5RJfFtA93r2.png, description=这是一段关于这个网站的描述文字 %}
{% endsitegroup %}

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}



# 单张图片 image

{% hideToggle Image %}

{% tabs test1 %}

<!-- tab 标签语法 -->

```markdown
{% image 链接, width=宽度（可选）, height=高度（可选）, alt=描述（可选）, bg=占位颜色（可选） %}
```

<!-- endtab -->

<!-- tab 示例源码 -->

```markdown
1. 指定宽度并添加描述：
{% image https://cdn.jsdelivr.net/gh/volantis-x/cdn-wallpaper-minimalist/2020/025.jpg, width=400px, alt=每天下课回宿舍的路，没有什么故事。 %}
2. 设置占位背景色：
{% image https://cdn.jsdelivr.net/gh/volantis-x/cdn-wallpaper-minimalist/2020/025.jpg, width=400px, bg=#1D0C04, alt=优化不同宽度浏览的观感 %}
```

<!-- endtab -->

<!-- tab 样式预览 -->

__指定宽度并添加描述__

{% image https://cdn.jsdelivr.net/gh/volantis-x/cdn-wallpaper-minimalist/2020/025.jpg, width=400px, alt=每天下课回宿舍的路，没有什么故事。 %}

__设置占位背景色__

{% image https://cdn.jsdelivr.net/gh/volantis-x/cdn-wallpaper-minimalist/2020/025.jpg, width=400px, bg=#1D0C04, alt=优化不同宽度浏览的观感 %}


<!-- endtab -->

{% endtabs %}

{% endhideToggle %}


# 音频 audio

{% hideToggle Audio %}

{% tabs test1 %}

<!-- tab 标签语法 -->

```markdown
{% audio 音频链接 %}
```

<!-- endtab -->

<!-- tab 示例源码 -->

```markdown
{% audio https://github.com/volantis-x/volantis-docs/releases/download/assets/Lumia1020.mp3 %}
```

<!-- endtab -->

<!-- tab 样式预览 -->

{% audio https://github.com/volantis-x/volantis-docs/releases/download/assets/Lumia1020.mp3 %}

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}


# 诗词标签 poem

{% hideToggle Poem %}

{% tabs test1 %}

<!-- tab 标签语法 -->

```markdown
{% poem [title],[author] %}
诗词内容
{% endpoem %}
```

<!-- endtab -->

<!-- tab 配置参数 -->

1. title：诗词标题
2. author：作者，可以不写

<!-- endtab -->

<!-- tab 示例源码 -->

```markdown
{% poem 水调歌头,苏轼 %}
丙辰中秋，欢饮达旦，大醉，作此篇，兼怀子由。
明月几时有？把酒问青天。
不知天上宫阙，今夕是何年？
我欲乘风归去，又恐琼楼玉宇，高处不胜寒。
起舞弄清影，何似在人间？

转朱阁，低绮户，照无眠。
不应有恨，何事长向别时圆？
人有悲欢离合，月有阴晴圆缺，此事古难全。
但愿人长久，千里共婵娟。
{% endpoem %}
```

<!-- endtab -->

<!-- tab 样式预览 -->

{% poem 水调歌头,苏轼 %}
丙辰中秋，欢饮达旦，大醉，作此篇，兼怀子由。
明月几时有？把酒问青天。
不知天上宫阙，今夕是何年？
我欲乘风归去，又恐琼楼玉宇，高处不胜寒。
起舞弄清影，何似在人间？

转朱阁，低绮户，照无眠。
不应有恨，何事长向别时圆？
人有悲欢离合，月有阴晴圆缺，此事古难全。
但愿人长久，千里共婵娟。
{% endpoem %}

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}


# 气泡注释 bubble

{% hideToggle Bubble %}

{% tabs test1 %}

<!-- tab 标签语法 -->

```markdown
{% bubble [content] , [notation] ,[background-color] %}
```

<!-- endtab -->

<!-- tab 配置参数 -->

1. content: 注释词汇
2. notation: 悬停显示的注解内容
3. background-color: 可选，气泡背景色。默认为“#71a4e3”

<!-- endtab -->

<!-- tab 示例源码 -->

```markdown
最近我学到了不少新玩意儿（虽然对很多大佬来说这些已经是旧技术了），比如CSS的{% bubble 兄弟相邻选择器,"例如 h1 + p{margin-top:50px;}" %}，{% bubble flex布局,"Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性","#ec5830" %}，{% bubble transform变换,"transform 属性向元素应用 2D 或 3D 转换。该属性允许我们对元素进行旋转、缩放、移动或倾斜。","#1db675" %}，animation的{% bubble 贝塞尔速度曲线,"贝塞尔曲线(Bézier curve)，又称贝兹曲线或贝济埃曲线，是应用于二维图形应用程序的数学曲线。一般的矢量图形软件通过它来精确画出曲线，贝兹曲线由线段与节点组成，节点是可拖动的支点，线段像可伸缩的皮筋","#de4489" %}写法，还有今天刚看到的{% bubble clip-path,"clip-path属性使用裁剪方式创建元素的可显示区域。区域内的部分显示，区域外的隐藏。","#868fd7" %}属性。这些对我来说很新颖的概念狠狠的冲击着我以前积累起来的设计思路。
```

<!-- endtab -->

<!-- tab 样式预览 -->

最近我学到了不少新玩意儿（虽然对很多大佬来说这些已经是旧技术了），比如CSS的{% bubble 兄弟相邻选择器,"例如 h1 + p {margin-top:50px;}" %}，{% bubble flex布局,"Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性","#ec5830" %}，{% bubble transform变换,"transform 属性向元素应用 2D 或 3D 转换。该属性允许我们对元素进行旋转、缩放、移动或倾斜。","#1db675" %}，animation的{% bubble 贝塞尔速度曲线,"贝塞尔曲线(Bézier curve)，又称贝兹曲线或贝济埃曲线，是应用于二维图形应用程序的数学曲线。一般的矢量图形软件通过它来精确画出曲线，贝兹曲线由线段与节点组成，节点是可拖动的支点，线段像可伸缩的皮筋","#de4489" %}写法，还有今天刚看到的{% bubble clip-path,"clip-path属性使用裁剪方式创建元素的可显示区域。区域内的部分显示，区域外的隐藏。","#868fd7" %}属性。这些对我来说很新颖的概念狠狠的冲击着我以前积累起来的设计思路。

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}


# 按钮 btns

{% tabs test1 %}

<!-- tab 标签语法 -->

```markdown
{% btns 样式参数 %}
{% cell 标题, 链接, 图片或者图标 %}
{% cell 标题, 链接, 图片或者图标 %}
{% endbtns %}
```

<!-- endtab -->

<!-- tab 配置参数 -->

1. 圆角样式：rounded, circle
2. 增加文字样式：可以在容器内增加 `<b>标题</b>`和`<p>描述文字</p>`
3. 布局方式：默认为自动宽度，适合视野内只有一两个的情况。

|参数	|含义|
| --- | --- |
|wide	|宽一点的按钮|
|fill	|填充布局，自动铺满至少一行，多了会换行|
|center	|居中，按钮之间是固定间距|
|around	|居中分散|
|grid2	|等宽最多2列，屏幕变窄会适当减少列数|
|grid3	|等宽最多3列，屏幕变窄会适当减少列数|
|grid4	|等宽最多4列，屏幕变窄会适当减少列数|
|grid5	|等宽最多5列，屏幕变窄会适当减少列数|

<!-- endtab -->

<!-- tab 示例源码 -->

1. 如果需要显示类似「团队成员」之类的一组含有头像的链接：
```markdown
{% btns circle grid5 %}
{% cell xaoxuu, https://xaoxuu.com, https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/avatar/avatar.png %}
{% cell xaoxuu, https://xaoxuu.com, https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/avatar/avatar.png %}
{% cell xaoxuu, https://xaoxuu.com, https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/avatar/avatar.png %}
{% cell xaoxuu, https://xaoxuu.com, https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/avatar/avatar.png %}
{% cell xaoxuu, https://xaoxuu.com, https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/avatar/avatar.png %}
{% endbtns %}
```
2. 或者含有图标的按钮：
```markdown
{% btns rounded grid5 %}
{% cell 下载源码, /, fas fa-download %}
{% cell 查看文档, /, fas fa-book-open %}
{% endbtns %}
```
3. 圆形图标 + 标题 + 描述 + 图片 + 网格5列 + 居中
```markdown
{% btns circle center grid5 %}
<a href='https://apps.apple.com/cn/app/heart-mate-pro-hrm-utility/id1463348922?ls=1'>
  <i class='fab fa-apple'></i>
  <b>心率管家</b>
  {% p red, 专业版 %}
  <img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/qrcode/heartmate_pro.png'>
</a>
<a href='https://apps.apple.com/cn/app/heart-mate-lite-hrm-utility/id1475747930?ls=1'>
  <i class='fab fa-apple'></i>
  <b>心率管家</b>
  {% p green, 免费版 %}
  <img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/qrcode/heartmate_lite.png'>
</a>
{% endbtns %}
```

<!-- endtab -->

<!-- tab 样式预览 -->

1. 如果需要显示类似「团队成员」之类的一组含有头像的链接：
{% btns circle grid5 %}
{% cell xaoxuu, https://xaoxuu.com, https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/avatar/avatar.png %}
{% cell xaoxuu, https://xaoxuu.com, https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/avatar/avatar.png %}
{% cell xaoxuu, https://xaoxuu.com, https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/avatar/avatar.png %}
{% cell xaoxuu, https://xaoxuu.com, https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/avatar/avatar.png %}
{% cell xaoxuu, https://xaoxuu.com, https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/avatar/avatar.png %}
{% endbtns %}
2. 或者含有图标的按钮：
{% btns rounded grid5 %}
{% cell 下载源码, /, fas fa-download %}
{% cell 查看文档, /, fas fa-book-open %}
{% endbtns %}
3. 圆形图标 + 标题 + 描述 + 图片 + 网格5列 + 居中
{% btns circle center grid5 %}
<a href='https://apps.apple.com/cn/app/heart-mate-pro-hrm-utility/id1463348922?ls=1'>
  <i class='fab fa-apple'></i>
  <b>心率管家</b>
  {% p red, 专业版 %}
  <img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/qrcode/heartmate_pro.png'>
</a>
<a href='https://apps.apple.com/cn/app/heart-mate-lite-hrm-utility/id1475747930?ls=1'>
  <i class='fab fa-apple'></i>
  <b>心率管家</b>
  {% p green, 免费版 %}
  <img src='https://cdn.jsdelivr.net/gh/xaoxuu/cdn-assets/qrcode/heartmate_lite.png'>
</a>
{% endbtns %}

<!-- endtab -->

{% endtabs %}


如有需要，请查阅其它笔记：
{% series Markdown的语法 %}