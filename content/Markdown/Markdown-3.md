---
title: Markdown的高级语法(1)
date: 2024-10-05 20:57
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
以下内容参考 [标签外挂](https://butterfly.js.org/posts/ceeb73f/)
仅供学习使用🎃🎃🎃
{% endnote %}

{% note info modern %}
标签外挂是 Hexo 独有的功能，并不是标准的 Markdown 格式。

以下的写法，只适用于 Butterfly 主题，用在其它主题上不会有效果，甚至可能会报错。使用前请留意

标签外挂虽然能为主题带来一些额外的功能和 UI 方面的强化，但是，标签外挂也有明显的限制，使用时请留意。
{% endnote %}

# Tabs

{% hideToggle Tabs %}

移植于 next 主题

使用方法：

```markdown
{% tabs Unique name, [index] %}

<!-- tab [Tab caption] [@icon] -->

Any content (support inline tags too).

<!-- endtab -->

{% endtabs %}
```

| 参数            | 解释                                                                                                |
| ------------- | ------------------------------------------------------------------------------------------------- |
| Unique name   | tabs 区块标签的唯一名称，不包含逗号。将用于每个选项卡的 # id 前缀，并附加其索引号。若名称中有空格，生成 # id 时会将空格替换为短横线。仅对当前文章/页面的 URL 必须唯一！   |
| [index]       | 【可选】活动选项卡的索引号。如果未指定，将选择第一个选项卡（1）。如果索引为 -1，则不会选择任何选项卡，类似于折叠内容。可选参数。                                |
| [Tab caption] | 当前选项卡的标题。如果未指定标题，将使用唯一名称和选项卡索引后缀作为选项卡标题。如果未指定标题，但指定了图标，标题将为空。                                     |
| [@icon]       | 【可选】FontAwesome 图标名称（全名，如 'fas fa-font'）。可以有或没有空格；例如 'Tab caption @icon' 与 'Tab caption@icon' 类似。 |

{% tabs test %}

<!-- tab 例子 1 - 预设选择第一个【默认】 -->

```markdown
{% tabs test1 %}

<!-- tab -->

**This is Tab 1.**

<!-- endtab -->

<!-- tab -->

**This is Tab 2.**

<!-- endtab -->

<!-- tab -->

**This is Tab 3.**

<!-- endtab -->

{% endtabs %}
```

{% tabs test1 %}

<!-- tab -->

**This is Tab 1.**

<!-- endtab -->

<!-- tab -->

**This is Tab 2.**

<!-- endtab -->

<!-- tab -->

**This is Tab 3.**

<!-- endtab -->

{% endtabs %}


<!-- endtab -->

<!-- tab 例子 2 - 预设选择 tabs -->

```markdown
{% tabs test2, 3 %}

<!-- tab -->

**This is Tab 1.**

<!-- endtab -->

<!-- tab -->

**This is Tab 2.**

<!-- endtab -->

<!-- tab -->

**This is Tab 3.**

<!-- endtab -->

{% endtabs %}
```

{% tabs test2, 3 %}

<!-- tab -->

**This is Tab 1.**

<!-- endtab -->

<!-- tab -->

**This is Tab 2.**

<!-- endtab -->

<!-- tab -->

**This is Tab 3.**

<!-- endtab -->

{% endtabs %}


<!-- endtab -->

<!-- tab 例子 3 - 没有预设值 -->

```markdown
{% tabs test3, -1 %}

<!-- tab -->

**This is Tab 1.**

<!-- endtab -->

<!-- tab -->

**This is Tab 2.**

<!-- endtab -->

<!-- tab -->

**This is Tab 3.**

<!-- endtab -->

{% endtabs %}
```

{% tabs test3, -1 %}

<!-- tab -->

**This is Tab 1.**

<!-- endtab -->

<!-- tab -->

**This is Tab 2.**

<!-- endtab -->

<!-- tab -->

**This is Tab 3.**

<!-- endtab -->

{% endtabs %}

<!-- endtab -->

<!-- tab 例子 4 - icon 和 Tab 名 -->

```markdown
{% tabs test4 %}

<!-- tab 第一个Tab -->

**tab 名字为第一个 Tab**

<!-- endtab -->

<!-- tab @fab fa-apple-pay -->

**只有图标 没有 Tab 名字**

<!-- endtab -->

<!-- tab 炸弹@fas fa-bomb -->

**名字+icon**

<!-- endtab -->

{% endtabs %}
```

{% tabs test4 %}

<!-- tab 第一个Tab -->

**tab 名字为第一个 Tab**

<!-- endtab -->

<!-- tab @fab fa-apple-pay -->

**只有图标 没有 Tab 名字**

<!-- endtab -->

<!-- tab 炸弹@fas fa-bomb -->

**名字+icon**

<!-- endtab -->

{% endtabs %}

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}

# Note (Bootstrap Callout)

{% hideToggle Note (Bootstrap Callout) %}

{% tabs test %}

<!-- tab 通用设置 -->

移植于 next 主题，并进行修改。

```yaml
note:
  # Note tag style values:
  #  - simple    bs-callout old alert style. Default.
  #  - modern    bs-callout new (v2-v3) alert style.
  #  - flat      flat callout style with background, like on Mozilla or StackOverflow.
  #  - disabled  disable all CSS styles import of note tag.
  style: simple
  icons: false
  border_radius: 3
  # Offset lighter of background in % for modern and flat styles (modern: -12 | 12; flat: -18 | 6).
  # Offset also applied to label tag variables. This option can work with disabled note tag.
  light_bg_offset: 0
```

|   参数  |   解释   |
|  ----   |   ----   |
|   style   |  【可选】标签样式（simple/modern/flat/disabled）   |
|  icons |  【可选】是否显示 icon  |
|   border_radius   |  【可选】边框圆角   |
|   light_bg_offset   |     【可选】背景色偏移量    |

Note 标签外挂有两种用法

`icons` 和 `light_bg_offset` 只对方法一生效

<!-- endtab -->

<!-- tab 用法 1 -->

```markdown
{% note [class] [no-icon] [style] %}
Any content (support inline tags too.io).
{% endnote %}
```

|   参数  |   解释   |
|  ----   |   ----   |
|   class   |  【可选】标识，不同的标识有不同的配色（ default / primary / success / info / warning / danger ）   |
|  no-icon |  【可选】不显示 icon  |
|   style   |  【可选】可以覆盖配置中的 style（simple/modern/flat/disabled）   |

{%tabs test%}

<!-- tab simple -->

```markdown
{% note simple %}
默认 提示块标签
{% endnote %}

{% note default simple %}
default 提示块标签
{% endnote %}

{% note primary simple %}
primary 提示块标签
{% endnote %}

{% note success simple %}
success 提示块标签
{% endnote %}

{% note info simple %}
info 提示块标签
{% endnote %}

{% note warning simple %}
warning 提示块标签
{% endnote %}

{% note danger simple %}
danger 提示块标签
{% endnote %}
```

{% note simple %}
默认 提示块标签
{% endnote %}

{% note default simple %}
default 提示块标签
{% endnote %}

{% note primary simple %}
primary 提示块标签
{% endnote %}

{% note success simple %}
success 提示块标签
{% endnote %}

{% note info simple %}
info 提示块标签
{% endnote %}

{% note warning simple %}
warning 提示块标签
{% endnote %}

{% note danger simple %}
danger 提示块标签
{% endnote %}

<!-- endtab -->

<!-- tab modern -->

```markdown
{% note modern %}
默认 提示块标签
{% endnote %}

{% note default modern %}
default 提示块标签
{% endnote %}

{% note primary modern %}
primary 提示块标签
{% endnote %}

{% note success modern %}
success 提示块标签
{% endnote %}

{% note info modern %}
info 提示块标签
{% endnote %}

{% note warning modern %}
warning 提示块标签
{% endnote %}

{% note danger modern %}
danger 提示块标签
{% endnote %}
```

{% note modern %}
默认 提示块标签
{% endnote %}

{% note default modern %}
default 提示块标签
{% endnote %}

{% note primary modern %}
primary 提示块标签
{% endnote %}

{% note success modern %}
success 提示块标签
{% endnote %}

{% note info modern %}
info 提示块标签
{% endnote %}

{% note warning modern %}
warning 提示块标签
{% endnote %}

{% note danger modern %}
danger 提示块标签
{% endnote %}

<!-- endtab -->

<!-- tab flat -->

```markdown
{% note flat %}
默认 提示块标签
{% endnote %}

{% note default flat %}
default 提示块标签
{% endnote %}

{% note primary flat %}
primary 提示块标签
{% endnote %}

{% note success flat %}
success 提示块标签
{% endnote %}

{% note info flat %}
info 提示块标签
{% endnote %}

{% note warning flat %}
warning 提示块标签
{% endnote %}

{% note danger flat %}
danger 提示块标签
{% endnote %}
```

{% note flat %}
默认 提示块标签
{% endnote %}

{% note default flat %}
default 提示块标签
{% endnote %}

{% note primary flat %}
primary 提示块标签
{% endnote %}

{% note success flat %}
success 提示块标签
{% endnote %}

{% note info flat %}
info 提示块标签
{% endnote %}

{% note warning flat %}
warning 提示块标签
{% endnote %}

{% note danger flat %}
danger 提示块标签
{% endnote %}

<!-- endtab -->

<!-- tab disabled -->

```markdown
{% note disabled %}
默认 提示块标签
{% endnote %}

{% note default disabled %}
default 提示块标签
{% endnote %}

{% note primary disabled %}
primary 提示块标签
{% endnote %}

{% note success disabled %}
success 提示块标签
{% endnote %}

{% note info disabled %}
info 提示块标签
{% endnote %}

{% note warning disabled %}
warning 提示块标签
{% endnote %}

{% note danger disabled %}
danger 提示块标签
{% endnote %}
```

{% note disabled %}
默认 提示块标签
{% endnote %}

{% note default disabled %}
default 提示块标签
{% endnote %}

{% note primary disabled %}
primary 提示块标签
{% endnote %}

{% note success disabled %}
success 提示块标签
{% endnote %}

{% note info disabled %}
info 提示块标签
{% endnote %}

{% note warning disabled %}
warning 提示块标签
{% endnote %}

{% note danger disabled %}
danger 提示块标签
{% endnote %}

<!-- endtab -->

<!-- tab no-icon -->

```markdown
{% note no-icon %}
默认 提示块标签
{% endnote %}

{% note default no-icon %}
default 提示块标签
{% endnote %}

{% note primary no-icon %}
primary 提示块标签
{% endnote %}

{% note success no-icon %}
success 提示块标签
{% endnote %}

{% note info no-icon %}
info 提示块标签
{% endnote %}

{% note warning no-icon %}
warning 提示块标签
{% endnote %}

{% note danger no-icon %}
danger 提示块标签
{% endnote %}
```

{% note no-icon %}
默认 提示块标签
{% endnote %}

{% note default no-icon %}
default 提示块标签
{% endnote %}

{% note primary no-icon %}
primary 提示块标签
{% endnote %}

{% note success no-icon %}
success 提示块标签
{% endnote %}

{% note info no-icon %}
info 提示块标签
{% endnote %}

{% note warning no-icon %}
warning 提示块标签
{% endnote %}

{% note danger no-icon %}
danger 提示块标签
{% endnote %}

<!-- endtab -->

{%endtabs%}


<!-- endtab -->

<!-- tab 用法 2(自定义icon) -->

{% note info modern %}
3.2.0 以上版本支持
{% endnote %}

```markdown
{% note [color] [icon] [style] %}
Any content (support inline tags too.io).
{% endnote %}
```

|   参数  |   解释   |
|  ----   |   ----   |
|   color   |  【可选】顔色(default / blue / pink / red / purple / orange / green)   |
|  icon |  【可选】可配置自定义 icon (只支持 fontawesome 图标, 也可以配置 no-icon )  |
|   style   |  【可选】可以覆盖配置中的 style（simple/modern/flat/disabled）   |

{%tabs test%}

<!-- tab simple -->

```markdown
{% note 'fab fa-cc-visa' simple %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' simple %}
2021 年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' simple %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' simple%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' simple %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' simple %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' simple %}
前端最讨厌的浏览器
{% endnote %}
```

{% note 'fab fa-cc-visa' simple %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' simple %}
2021 年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' simple %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' simple%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' simple %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' simple %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' simple %}
前端最讨厌的浏览器
{% endnote %}

<!-- endtab -->

<!-- tab modern -->

```markdown
{% note 'fab fa-cc-visa' modern %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' modern %}
2021 年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' modern %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' modern%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' modern %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' modern %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' modern %}
前端最讨厌的浏览器
{% endnote %}
```

{% note 'fab fa-cc-visa' modern %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' modern %}
2021 年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' modern %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' modern%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' modern %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' modern %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' modern %}
前端最讨厌的浏览器
{% endnote %}

<!-- endtab -->

<!-- tab flat -->

```markdown
{% note 'fab fa-cc-visa' flat %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' flat %}
2021 年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' flat %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' flat%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' flat %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' flat %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' flat %}
前端最讨厌的浏览器
{% endnote %}
```

{% note 'fab fa-cc-visa' flat %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' flat %}
2021 年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' flat %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' flat%}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' flat %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' flat %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' flat %}
前端最讨厌的浏览器
{% endnote %}

<!-- endtab -->

<!-- tab disabled -->

```markdown
{% note 'fab fa-cc-visa' disabled %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' disabled %}
2021 年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' disabled %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' disabled %}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' disabled %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' disabled %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' disabled %}
前端最讨厌的浏览器
{% endnote %}
```

{% note 'fab fa-cc-visa' disabled %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue 'fas fa-bullhorn' disabled %}
2021 年快到了....
{% endnote %}
{% note pink 'fas fa-car-crash' disabled %}
小心开车 安全至上
{% endnote %}
{% note red 'fas fa-fan' disabled %}
这是三片呢？还是四片？
{% endnote %}
{% note orange 'fas fa-battery-half' disabled %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple 'far fa-hand-scissors' disabled %}
剪刀石头布
{% endnote %}
{% note green 'fab fa-internet-explorer' disabled %}
前端最讨厌的浏览器
{% endnote %}

<!-- endtab -->

<!-- tab no-icon -->

```markdown
{% note no-icon %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue no-icon %}
2021 年快到了....
{% endnote %}
{% note pink no-icon %}
小心开车 安全至上
{% endnote %}
{% note red no-icon %}
这是三片呢？还是四片？
{% endnote %}
{% note orange no-icon %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple no-icon %}
剪刀石头布
{% endnote %}
{% note green no-icon %}
前端最讨厌的浏览器
{% endnote %}
```

{% note no-icon %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note blue no-icon %}
2021 年快到了....
{% endnote %}
{% note pink no-icon %}
小心开车 安全至上
{% endnote %}
{% note red no-icon %}
这是三片呢？还是四片？
{% endnote %}
{% note orange no-icon %}
你是刷 Visa 还是 UnionPay
{% endnote %}
{% note purple no-icon %}
剪刀石头布
{% endnote %}
{% note green no-icon %}
前端最讨厌的浏览器
{% endnote %}

<!-- endtab -->

{%endtabs%}

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}

# Mermaid

{% hideToggle Mermaid %}

使用 mermaid 标签可以绘制 Flowchart（流程图）、Sequence diagram（时序图 ）、Class Diagram（类别图）、State Diagram（状态图）、Gantt（甘特图）和 Pie Chart（圆形图），具体可以查看[mermaid 文档](https://mermaid-js.github.io/mermaid/#/)

```yaml
# Mermaid
# https://github.com/mermaid-js/mermaid
mermaid:
  enable: false
  # Write Mermaid diagrams using code blocks
  code_write: false
  # built-in themes: default / forest / dark / neutral
  theme:
    light: default
    dark: dark
```

写法：

```markdown
{% mermaid %}
内容
{% endmermaid %}
```

例子：

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

{% mermaid %}
pie
title Key elements in Product X
"Calcium" : 42.96
"Potassium" : 50.05
"Magnesium" : 10.01
"Iron" : 5
{% endmermaid %}

{% endhideToggle %}

# Button

{% hideToggle Button %}

{% note info modern %}
3.0 以上适用
{% endnote %}

使用方法：

```markdown
{% btn [url],[text],[icon],[color] [style] [layout] [position] [size] %}
```

| 参数 |	解释  |
| --- | ---  |
| url	|   【必须】链接地址  |
|  text	|  【必须】按钮文字  |
|   icon  |	【可选】图标   |
|  color	| 【可选】按钮背景顔色（默认style时）按钮字体和边框顔色(outline 时)<br>配置：default/blue/pink/red/purple/orange/green   |
| style	|  【可选】按钮样式 默认实心<br>配置： outline/留空   |
|  layout	|  【可选】按钮佈局 默认为 line<br>配置： block/留空   |
|  position	|  【可选】按钮位置 前提是设置了layout为block默认为左边<br>配置：center/right/留空   |
|  size	|  【可选】按钮大小<br>配置：larger/留空   |

{%tabs text%}

<!-- tab 例 1 -->

```markdown
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,larger %}
```

This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,larger %}


<!-- endtab -->

<!-- tab 例 2 -->

```markdown
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block center larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block right outline larger %}
```

{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block center larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,block right outline larger %}

<!-- endtab -->

<!-- tab 例 3 -->

```markdown
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,blue larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,pink larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,red larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,purple larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,orange larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,green larger %}
```

<div class="btn-center">
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,blue larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,pink larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,red larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,purple larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,orange larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,green larger %}
</div>

<!-- endtab -->

<!-- tab 例 4 -->

```markdown
<div class="btn-center">
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline blue larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline pink larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline red larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline purple larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline orange larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline green larger %}
</div>
```

<div class="btn-center">
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline blue larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline pink larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline red larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline purple larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline orange larger %}
{% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline green larger %}
</div>

<!-- endtab -->

{%endtabs%}

{% endhideToggle %}

# InlineImg

{% hideToggle InlineImg %}

主题中的图片都是默认以`块级元素`显示，如果你想以`内联元素`显示，可以使用这个标签外挂。

```markdown
{% inlineImg [src] [height] %}
```

| 参数	|  解释  |
| --- | ---   |
|  src	|  图片链接  |
| height	| 【可选】图片高度限制  |

例子：

```markdown
你看她长得漂亮不

![](https://github.com/Anthony-hcy/Tuchuang/blob/main/Image/Wallpaper/A%20(2).png?raw=true)

我觉得很漂亮 {% inlineImg https://i.loli.net/2021/03/19/5M4jUB3ynq7ePgw.png 150px %}
```

你看她长得漂亮不

<img align="center" src="https://github.com/Anthony-hcy/Tuchuang/blob/main/Image/Wallpaper/A%20(2).png?raw=true"  width="400"/>

我觉得很漂亮 {% inlineImg https://i.loli.net/2021/03/19/5M4jUB3ynq7ePgw.png 100px %}

{% endhideToggle %}

# Label

{% hideToggle Label %}

{% note info modern %}
由于 hexo 的渲染限制， 在段落开头使用 label 标签外挂会出现一些问题。例如：连续开头使用 label 标签外挂的段落无法换行

建议不要在段落开头使用 label 标签外挂
{% endnote %}

```markdown
{% label text color %}
```

|参数	|解释|
| ---  | --- |
|text	|文字|
|color	|【可选】背景颜色，默认为`default`<br>default/blue/pink/red/purple/orange/green

例子：

```markdown
臣亮言：{% label 先帝 %}创业未半，而{% label 中道崩殂 blue %}。今天下三分，{% label 益州疲敝 pink %}，此诚{% label 危急存亡之秋 red %}也！然侍衞之臣，不懈于内；{% label 忠志之士 purple %}，忘身于外者，盖追先帝之殊遇，欲报之于陛下也。诚宜开张圣听，以光先帝遗德，恢弘志士之气；不宜妄自菲薄，引喻失义，以塞忠谏之路也。

宫中、府中，俱为一体；陟罚臧否，不宜异同。若有{% label 作奸 orange %}、{% label 犯科 green %}，及为忠善者，宜付有司，论其刑赏，以昭陛下平明之治；不宜偏私，使内外异法也。
```

臣亮言：{% label 先帝 %}创业未半，而{% label 中道崩殂 blue %}。今天下三分，{% label 益州疲敝 pink %}，此诚{% label 危急存亡之秋 red %}也！然侍衞之臣，不懈于内；{% label 忠志之士 purple %}，忘身于外者，盖追先帝之殊遇，欲报之于陛下也。诚宜开张圣听，以光先帝遗德，恢弘志士之气；不宜妄自菲薄，引喻失义，以塞忠谏之路也。

宫中、府中，俱为一体；陟罚臧否，不宜异同。若有{% label 作奸 orange %}、{% label 犯科 green %}，及为忠善者，宜付有司，论其刑赏，以昭陛下平明之治；不宜偏私，使内外异法也。

{% endhideToggle %}

# Timeline

{% hideToggle Timeline %}

{% note info modern %}
4.0.0 以上支持
{% endnote %}

```markdown
{% timeline title,color %}

<!-- timeline title -->

xxxxx

<!-- endtimeline -->
<!-- timeline title -->

xxxxx

<!-- endtimeline -->

{% endtimeline %}
```

|参数	|解释|
| --- | --- |
|title	|标题/时间线|
|color	|timeline 颜色<br>default(留空) / blue / pink / red / purple / orange / green|

{%tabs text%}

<!-- tab default -->

```markdown
{% timeline 2022 %}

<!-- timeline 01-02 -->

这是测试页面

<!-- endtimeline -->

{% endtimeline %}
```

{% timeline 2022 %}

<!-- timeline 01-02 -->

这是测试页面

<!-- endtimeline -->

{% endtimeline %}


<!-- endtab -->

<!-- tab blue -->

```markdown
{% timeline 2022,blue %}

<!-- timeline 01-02 -->

这是测试页面

<!-- endtimeline -->

{% endtimeline %}
```

{% timeline 2022,blue %}

<!-- timeline 01-02 -->

这是测试页面

<!-- endtimeline -->

{% endtimeline %}

<!-- endtab -->

<!-- tab pink -->

```markdown
{% timeline 2022,pink %}

<!-- timeline 01-02 -->

这是测试页面

<!-- endtimeline -->

{% endtimeline %}
```

{% timeline 2022,pink %}

<!-- timeline 01-02 -->

这是测试页面

<!-- endtimeline -->

{% endtimeline %}

<!-- endtab -->

<!-- tab red -->

```markdown
{% timeline 2022,red %}

<!-- timeline 01-02 -->

这是测试页面

<!-- endtimeline -->

{% endtimeline %}
```

{% timeline 2022,red %}

<!-- timeline 01-02 -->

这是测试页面

<!-- endtimeline -->

{% endtimeline %}

<!-- endtab -->

<!-- tab purple -->

```markdown
{% timeline 2022,purple %}

<!-- timeline 01-02 -->

这是测试页面

<!-- endtimeline -->

{% endtimeline %}
```

{% timeline 2022,purple %}

<!-- timeline 01-02 -->

这是测试页面

<!-- endtimeline -->

{% endtimeline %}

<!-- endtab -->

<!-- tab orange -->

```markdown
{% timeline 2022,orange %}

<!-- timeline 01-02 -->

这是测试页面

<!-- endtimeline -->

{% endtimeline %}
```

{% timeline 2022,orange %}

<!-- timeline 01-02 -->

这是测试页面

<!-- endtimeline -->

{% endtimeline %}

<!-- endtab -->

<!-- tab green -->

```markdown
{% timeline 2022,green %}

<!-- timeline 01-02 -->

这是测试页面

<!-- endtimeline -->

{% endtimeline %}
```

{% timeline 2022,green %}

<!-- timeline 01-02 -->

这是测试页面

<!-- endtimeline -->

{% endtimeline %}

<!-- endtab -->

{%endtabs%}

{% endhideToggle %}

# ABCJS 乐谱

{% hideToggle ABCJS %}

在页面上渲染乐谱

```yaml
# abcjs (乐谱渲染)
# See https://github.com/paulrosen/abcjs
# ---------------
abcjs:
  enable: true
  per_page: true
```

|参数	|解释|
| --- | --- |
|enable	|是否启用 ABCJS|
|per_page	|是否每页都加载 ABCJS, 如果设爲 false, 在你使用 ABCJS 时，你需要在使用 ABCJS 的页面 Front-matter 添加 `abcjs: true` |

写法：

```markdown
{% score %}
乐谱代码
{% endscore %}
```

例子：

```markdown
{% score %}
X:1
T:alternate heads
M:C
L:1/8
U:n=!style=normal!
K:C treble style=rhythm
"Am" BBBB B2 B>B | "Dm" B2 B/B/B "C" B4 |"Am" B2 nGnB B2 nGnA | "Dm" nDB/B/ nDB/B/ "C" nCB/B/ nCB/B/ |B8| B0 B0 B0 B0 |]
%%text This translates to:
[M:C][K:style=normal]
[A,EAce][A,EAce][A,EAce][A,EAce] [A,EAce]2 [A,EAce]>[A,EAce] |[DAdf]2 [DAdf]/[DAdf]/[DAdf] [CEGce]4 |[A,EAce]2 GA [A,EAce] GA |D[DAdf]/[DAdf]/ D[DAdf]/[DAdf]/ C [CEGce]/[CEGce]/ C[CEGce]/[CEGce]/ |[CEGce]8 | [CEGce]2 [CEGce]2 [CEGce]2 [CEGce]2 |]
GAB2 !style=harmonic![gb]4|GAB2 [K: style=harmonic]gbgb|
[K: style=x]
C/A,/ C/C/E C/zz2|
w:Rock-y did-nt like that
{% endscore %}
```

{% score %}
X:1
T:alternate heads
M:C
L:1/8
U:n=!style=normal!
K:C treble style=rhythm
"Am" BBBB B2 B>B | "Dm" B2 B/B/B "C" B4 |"Am" B2 nGnB B2 nGnA | "Dm" nDB/B/ nDB/B/ "C" nCB/B/ nCB/B/ |B8| B0 B0 B0 B0 |]
%%text This translates to:
[M:C][K:style=normal]
[A,EAce][A,EAce][A,EAce][A,EAce] [A,EAce]2 [A,EAce]>[A,EAce] |[DAdf]2 [DAdf]/[DAdf]/[DAdf] [CEGce]4 |[A,EAce]2 GA [A,EAce] GA |D[DAdf]/[DAdf]/ D[DAdf]/[DAdf]/ C [CEGce]/[CEGce]/ C[CEGce]/[CEGce]/ |[CEGce]8 | [CEGce]2 [CEGce]2 [CEGce]2 [CEGce]2 |]
GAB2 !style=harmonic![gb]4|GAB2 [K: style=harmonic]gbgb|
[K: style=x]
C/A,/ C/C/E C/zz2|
w:Rock-y did-nt like that
{% endscore %}

{% endhideToggle %}

# Series 系列文章

{% hideToggle Series %}

在页面上显示系列文章

```yaml
series:
  enable: true
  orderBy: 'title' # Order by title or date
  order: 1 # Sort of order. 1, asc for ascending; -1, desc for descending
  number: true
```

|参数	|解释|
| --- | --- |
|enable	|是否启用 series|
|orderBy	|排序方式，默认为 title, 可选 title / date|
|order	|排序方式，默认为 1, 可选 1 (升序) / -1（降序）|
|number	|显示序列号|

写法：

```markdown
{% series %}
{% series [series name] %}
```

在文章的`front-matter`上添加参数 series，并给与一个标识

使用此标签外挂，会把相同标识的文章以列表的形式展示

如果不写 series 标识，则默认为你使用此标签外挂所在的文章的 series 标识

例子：

```markdown
---
series: Markdown的语法
---

如有需要，请查阅其它笔记：
{% series Markdown的语法 %}
```

如有需要，请查阅其它笔记：
{% series Markdown的语法 %}

{% endhideToggle %}

# Gallery 相册图库

{% hideToggle Gallery %}

{% tabs text %}

<!-- tab 图库 -->
一个图库集合。

写法：

```markdown
<div class="gallery-group-main">
{% galleryGroup name description link img-url %}
{% galleryGroup name description link img-url %}
{% galleryGroup name description link img-url %}
</div>
```

|参数	|解释|
| --- | --- |
|name	|图库名字|
|description	|图库描述|
|link	|连接到对应相册的地址|
|img-url	|图库封面的地址|

例子：

```markdown
<div class="gallery-group-main">

{% galleryGroup '封面专区' '本站用作文章封面的图片，不保证分辨率' '/life/gallery/img' https://github.com/Anthony-hcy/Tuchuang/blob/main/Image/Wallpaper/A%20(3).png?raw=true %}

{% galleryGroup '背景专区' '收藏的一些的背景与壁纸，分辨率很高' '/life/gallery/wallpaper' https://github.com/Anthony-hcy/Tuchuang/blob/main/Image/Wallpaper/A%20(36).png?raw=true %}
</div>
```

<div class="gallery-group-main">

{% galleryGroup '封面专区' '本站用作文章封面的图片，不保证分辨率' '/life/gallery/img' https://source.fomal.cc/img/default_cover_61.webp %}

{% galleryGroup '背景专区' '收藏的一些的背景与壁纸，分辨率很高' '/life/gallery/wallpaper' https://source.fomal.cc/img/dm11.webp %}
</div>

<!-- endtab -->

<!-- tab 相册 -->

区别于旧版的 Gallery 相册,新的 Gallery 相册会自动根据图片长度进行排版，书写也更加方便，与 markdown 格式一样。可根据需要插入到相应的 md。

{% tabs t %}

<!-- tab 本地 -->

写法;

```markdown
{% gallery [lazyload],[rowHeight],[limit] %}
markdown 图片格式
{% endgallery %}
```

|参数	|解释|
| --- | --- |
|lazyload	|【可选】点击按钮加载更多图片，填写 true/false。默认为 `false`。|
|rowHeight	|【可选】图片显示的高度，如果需要一行显示更多的图片，可设置更小的数字。默认为 `220`。|
|limit|	【可选】每次加载多少张照片。默认为 `10`。|

例子：

```markdown
{% gallery %} 
![]( https://github.com/Anthony-hcy/Tuchuang/blob/main/Image/Wallpaper/A%20(32).png?raw=true ) 
![]( https://github.com/Anthony-hcy/Tuchuang/blob/main/Image/Wallpaper/A%20(33).png?raw=true ) 
![]( https://github.com/Anthony-hcy/Tuchuang/blob/main/Image/Wallpaper/A%20(34).png?raw=true ) 
![]( https://github.com/Anthony-hcy/Tuchuang/blob/main/Image/Wallpaper/A%20(35).png?raw=true ) 
![]( https://github.com/Anthony-hcy/Tuchuang/blob/main/Image/Wallpaper/A%20(36).png?raw=true ) 
![]( https://github.com/Anthony-hcy/Tuchuang/blob/main/Image/Wallpaper/A%20(37).png?raw=true ) 
![]( https://github.com/Anthony-hcy/Tuchuang/blob/main/Image/Wallpaper/A%20(38).png?raw=true ) 
![]( https://github.com/Anthony-hcy/Tuchuang/blob/main/Image/Wallpaper/A%20(39).png?raw=true ) 
![]( https://github.com/Anthony-hcy/Tuchuang/blob/main/Image/Wallpaper/A%20(40).png?raw=true ) 
![]( https://github.com/Anthony-hcy/Tuchuang/blob/main/Image/Wallpaper/A%20(41).png?raw=true ) 
{% endgallery %}
```

{% gallery true,150,10 %} 
![A-1.png](https://tuchuang.voooe.cn/images/2024/10/14/A-1.png) 
![A-2.png](https://tuchuang.voooe.cn/images/2024/10/14/A-2.png) 
![A-3.png](https://tuchuang.voooe.cn/images/2024/10/14/A-3.png)  
![A-4.png](https://tuchuang.voooe.cn/images/2024/10/14/A-4.png) 
![A-5.png](https://tuchuang.voooe.cn/images/2024/10/14/A-5.png) 
![A-6.png](https://tuchuang.voooe.cn/images/2024/10/14/A-6.png)  
![A-7.png](https://tuchuang.voooe.cn/images/2024/10/14/A-7.png) 
![A-8.png](https://tuchuang.voooe.cn/images/2024/10/14/A-8.png) 
![A-9.png](https://tuchuang.voooe.cn/images/2024/10/14/A-9.png)  
![A-10.png](https://tuchuang.voooe.cn/images/2024/10/14/A-10.png) 
![A-11.png](https://tuchuang.voooe.cn/images/2024/10/14/A-11.png) 
![A-12.png](https://tuchuang.voooe.cn/images/2024/10/14/A-12.png)  
![A-13.png](https://tuchuang.voooe.cn/images/2024/10/14/A-13.png) 
![A-14.png](https://tuchuang.voooe.cn/images/2024/10/14/A-14.png) 
![A-15.png](https://tuchuang.voooe.cn/images/2024/10/14/A-15.png)  
![A-16.png](https://tuchuang.voooe.cn/images/2024/10/14/A-16.png) 
![A-17.png](https://tuchuang.voooe.cn/images/2024/10/14/A-17.png) 
![A-18.png](https://tuchuang.voooe.cn/images/2024/10/14/A-18.png)  
![A-19.png](https://tuchuang.voooe.cn/images/2024/10/14/A-19.png)
![A-20.png](https://tuchuang.voooe.cn/images/2024/10/14/A-20.png)
![A-21.png](https://tuchuang.voooe.cn/images/2024/10/14/A-21.png)
![A-22.png](https://tuchuang.voooe.cn/images/2024/10/14/A-22.png)
![A-23.png](https://tuchuang.voooe.cn/images/2024/10/14/A-23.png)
![A-24.png](https://tuchuang.voooe.cn/images/2024/10/14/A-24.png)
![A-25.png](https://tuchuang.voooe.cn/images/2024/10/14/A-25.png)
![A-26.png](https://tuchuang.voooe.cn/images/2024/10/14/A-26.png)
![A-27.png](https://tuchuang.voooe.cn/images/2024/10/14/A-27.png)
![A-28.png](https://tuchuang.voooe.cn/images/2024/10/14/A-28.png)
![A-29.png](https://tuchuang.voooe.cn/images/2024/10/14/A-29.png)
![A-30.png](https://tuchuang.voooe.cn/images/2024/10/14/A-30.png)
{% endgallery %}

<!-- endtab -->

<!-- tab 远程拉取 -->

写法：

```markdown
{% gallery url,[link],[lazyload],[rowHeight],[limit] %}
{% endgallery %}
```

|参数	|解释|
| --- | --- |
|url	|【必须】 识别词|
|link	|【必须】远程的 json 链接|
|lazyload|	【可选】点击按钮加载更多图片，填写 true/false。默认为 `false`。|
|rowHeight|	【可选】图片显示的高度，如果需要一行显示更多的图片，可设置更小的数字。默认为 `220`。|
|limit|	【可选】每次加载多少张照片。默认为 `10`。|

有三个参数，`url`是必须存在的，`alt` 和 `title` 可有，也可没有。

```json
[
  {
    "url": "https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/IMG_0556.jpg",
    "alt": "IMG_0556.jpg",
    "title": "这是title"
  },
  {
    "url": "https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/IMG_0472.jpg",
    "alt": "IMG_0472.jpg"
  },
  {
    "url": "https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/IMG_0453.jpg",
    "alt": ""
  },
  {
    "url": "https://cdn.jsdelivr.net/gh/jerryc127/CDN/img/IMG_0931.jpg",
    "alt": ""
  }
]
```

```markdown
{% gallery url,https://xxxx.com/sss.json %}
{% endgallery %}

{% gallery url,https://xxxx.com/sss.json,true,220,10 %}
{% endgallery %}

{% gallery url,https://xxxx.com/sss.json,true,,10 %}
{% endgallery %}
```

<!-- endtab -->

{% endtabs %}

<!-- endtab -->

{% endtabs %}

{% endhideToggle %}

# Tag-hide
---

{% note info modern %}
2.2.0 以上提供
请注意，tag-hide 内的标签外挂 content 内都不建议有 h1 - h6 等标题。因为 Toc 会把隐藏内容标题也显示出来，而且当滚动屏幕时，如果隐藏内容没有显示出来，会导致 Toc 的滚动出现异常。
{% endnote %}

如果你想把一些文字、内容隐藏起来，并提供按钮让用户点击显示。可以使用这个标签外挂。

{% tabs test %}

<!-- tab Inline -->

`inline` 在文本里面添加按钮隐藏内容，只限文字

( content 不能包含英文逗号，可用`&sbquo;`)

```markdown
{% hideInline content,display,bg,color %}
```

|   参数  |   解释   |
|  ----   |   ----   |
|   content   |  文本内容  |
|  display |  【可选】按钮显示的文字  |
|   bg   |  【可选】按钮的背景颜色   |
|    color    |       【可选】按钮文字的颜色              |

{% note default modern %}
例子
{% endnote %}

```markdown
哪个英文字母最酷？ {% hideInline 因为西装裤(C装酷),查看答案,#FF7242,#fff %}

门里站着一个人? {% hideInline 闪 %}
```

哪个英文字母最酷？ 
{% hideBlock 查看答案,#FF7242,#fff %}
因为西装裤(C装酷)
{% endhideBlock%}
门里站着一个人？  
{% hideBlock Click %}
闪
{%endhideBlock%}

<!-- endtab -->

<!-- tab Block -->

`block`独立的 block 隐藏内容，可以隐藏很多内容，包括图片，代码块等等

( display 不能包含英文逗号，可用`&sbquo;`)

```markdown
{% hideBlock display,bg,color %}
content
{% endhideBlock %}
```

|   参数  |   解释   |
|  ----   |   ----   |
|   content   |  文本内容  |
|  display |  【可选】按钮显示的文字  |
|   bg   |  【可选】按钮的背景颜色   |
|    color    |       【可选】按钮文字的颜色              |

{% note default modern %}
例子
{% endnote %}

```markdown
查看答案
{% hideBlock 查看答案 %}
傻子，怎么可能有答案
{% endhideBlock %}
```

查看答案
{% hideBlock 查看答案 %}
傻子，怎么可能有答案
{% endhideBlock %}


<!-- endtab -->

<!-- tab Toggle -->

{% note info modern %}
2.3.0 以上支持
{% endnote %}

如果你需要展示的内容太多，可以把它隐藏在收缩框里，需要时再把它展开。

( display 不能包含英文逗号，可用`&sbquo;`)

```markdown
{% hideToggle display,bg,color %}
content
{% endhideToggle %}
```

|   参数  |   解释   |
|  ----   |   ----   |
|  display |  显示的文字  |
|   bg   |  【可选】背景颜色   |
|    color    |  【可选】文字的颜色     |

{% note default modern %}
例子
{% endnote %}

```markdown
{% hideToggle Butterfly安装方法 %}
在你的博客根目录里

git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly

如果想要安装比较新的 dev 分支，可以

git clone -b dev https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly

{% endhideToggle %}

```

{% hideToggle Butterfly安装方法 %}
在你的博客根目录里

git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly

如果想要安装比较新的 dev 分支，可以

git clone -b dev https://github.com/jerryc127/hexo-theme-butterfly.git themes/Butterfly

{% endhideToggle %}


<!-- endtab -->

{% endtabs %}

如有需要，请查阅其它笔记：
{% series Markdown的语法 %}