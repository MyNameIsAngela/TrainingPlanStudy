#《CSS权威指南》读书笔记#

**【2017-11-19 】 **

##第一章 CSS和文档##

###1.1.CSS和HTML的结合###

- link标记 || 外部样式表

  link必须放在head元素中，因为它们是html文档外部的样式表。

```html
<link rel ="stylesheet" type="text/css" href="sheet1.css" media ="all" />
```

- style元素 || 文档样式表 || 嵌套样式表

  一定要使用type属性。

```html
<style type="text/css">
...
</style>
```

- @import指令

  加载一个外部样式表，与link类似，但区别在于命令的具体语法和位置。@import出现在style容器中，且要放在其他css规则之前，否则会被忽略。（个人认为此指令方便样式表的嵌套使用）

```html
<style type="text/cee">
@import url(sheet2.css);
@import url(sheet3.css) all, screen;
h1 {color:gray};
</style>
```

- 内联样式

```html
<p style="color:gray; background:yellow">
  inline style
</p>
```

##第二章 选择器##

###2.1.通配选择器（*）、元素选择器（p）、类选择器（.）、id选择器（#）###

```html
//例子
* {... }
p.warning{...}
.warning.urgent{...}//两个类选择器链接在一起，仅可以选择同时包含这些类名的元素（类名的顺序不限）
```

###2.2.属性选择器###

- 简单属性选择

  希望选择有某个属性的元素，不论该属性的值是什么。例如，`h1[class]{...}`选择有class属性（值不限）的所有h1元素。允许多个属性选择器链接，例如，`a[href][title]`同时设置有href和title属性的超链接。


- 根据具体属性值选择（=）完全串匹配

  进一步缩小选择范围，只选择有特定属性值的元素。例如，`h1[class="warning"]`选择class属性值为warning的那些h1元素。允许多个属性-值选择器链接，例如，`h1[class="warning"][title="head"]`选择class值为warning且title值为head的所有h1元素。

  注意⚠️：id选择器与指定id属性的属性选择器不是一回事。`h1#page-title`与`h1[id="page-title"]`存在微妙但很重要的差别。


- 根据部分属性值选择  子串匹配属性选择器

  | 类型           | 描述                             |
  | ------------ | ------------------------------ |
  | [foo~="bar"] | 选择foo属性中包含bar（且bar为用空格分隔）的所有元素 |
  | [foo^="bar"] | 选择foo属性中以“bar”开头的所有元素          |
  | [foo$="bar"] | 选择foo属性中以“bao”结尾的所有元素          |
  | [foo*="bar"] | 选择foo属性中包含子串bar的所有元素           |

- 特定属性选择类型

  `img[src|="fig"]`选择src属性等于fig或以fig-开头的所有元素，例如可以匹配形如fig-1.gif、fig-2.gif

**总结："value是完整单词" 类型的比较符号:  ~=  ,  |=；"拼接字符串" 类型的比较符号:  *=  ,  ^=  ,  $=。**

###2.3.文档结构###

- 后代选择器

  `h1 em {color:gray;}`选择作为h1元素后代的任何em元素

- 选择子元素

  `h1 > strong {color:red;}`选择只作为一个h1元素子元素（而不是后代元素）的strong元素。

- 选择相邻兄弟元素

  `h1 + p {margin-top: 0;}`选择紧接在一个h1元素后出现的所有段落，h1要与p元素有共同的父元素。

###2.4.伪类和伪元素###

1.伪类选择器 

为文档中不一定具体存在的结构指定样式。把某种幻像类关联到与伪类相关的元素，使得xx中好像已经有一个xx类一样。

```html
li:first-child
//相当于
<li class="first-child">insert key</li>	
<li>turn key</li>	
<li>push accelerator</li>	

```

| 伪类   | 伪类名          | 描述                                       |
| ---- | :----------- | ---------------------------------------- |
| 链接伪类 | :link        | 指示作为超链接（有href属性）的所有锚                     |
|      | :visited     | 指示作为已访问地址超链接的所有锚                         |
| 动态伪类 | :focus       | 指示当前拥有输入焦点的元素，如可以接受键盘输入                  |
|      | :hover       | 指示鼠标指针停留在哪个元素上                           |
|      | :active      | 指示被用户输入激活的元素                             |
| 静态伪类 | :first-child | 选择元素的第一个子元素                              |
|      | :lang()      | 选择某种语言，如`:lang(fr){...}`选择所有法语元素         |
| 结合伪类 |              | 可以在同一个选择器中结合使用伪类，例如`a:link:hover {...}`，结合时哪种顺序指定不重要，但不要把互斥的伪类结合在一起使用，例如`a:link:visited {...}`。 |

注意⚠️：伪类的顺序很重要，通常的建议是 link-visited-focus-hover-active.

2.伪元素选择器

在文档中插入假象的元素，从而得到某种效果。所有伪元素必须放在该伪元素的选择器的后面，例如`p:first-line em`是不合法的。

```html
h2:first-letter{...}
相当于
<h2>
  <h2:first-letter>T</h2:first-letter>his is an h2 element
</h2>
```

| 伪元素           | 解释      | 备注                                 |
| ------------- | ------- | ---------------------------------- |
| :first-letter | 设置首字母样式 |                                    |
| :first-line   | 设置第一行样式 |                                    |
| :before       | 设置之前样式  | 例如`h2:before{content:"<插入内容>";...} |
| :after        | 设置之后样式  |                                    |

注意⚠️：:first-letter和:first-line伪元素只能应用于标记或段落之类的块元素。具体使用时应再检查其所允许的属性。

**【第二章练习代码： <https://jsbin.com/tiqaduz/edit?html,css,output>】**

##第三章 结构和层叠##

###3.1特殊性###

每个元素都有它的特殊性，如果一个元素有两个或多个冲突的属性声明，那么有最高特殊性的声明就会胜出。

| 特殊性        | 说明                                       |
| ---------- | ---------------------------------------- |
| ！important | 声明重要性超过了所有其他声明。！important总是放在声明的最后，即分号前面。如`p{color: #333 !important; background:red !important;}`。！important声明没有特殊的特殊性值，它的冲突会在重要声明内部解决，而不会与非重要声明相混。 |
| 1000       | 內联样式，第一个0位是为内联样式声明保留的，它比所有其他声明的特殊性都高。但在css2中，内联样式特殊性=ID选择器特殊性 |
| 0100       | 【ID选择器】                                  |
| 0010       | 【类选择器】【属性选择】【伪类】                         |
| 0001       | 【元素】【伪元素】                                |
| 0000       | 【结合符】【通配选择器】对特殊性没有任何贡献。                  |
| Null       | 继承的值                                     |

###3.2继承###

样式不仅应用到指定的元素，还会应用到它的后代元素。例如color属性会沿着文档书向下传播到后代元素，但绝不会向上传播。且有些属性，如border，不能继承。继承的值没有特殊性，甚至连0特殊性都没有。（0特殊性比无特殊性要强）

###3.3层叠###

解决特殊性相等的两个规则应选哪一个的问题。

1. 按来源：important读者样式>important创作人员样式>创作人员样式>读者的样式>用户代理样式
2. 特殊性高>特殊性低
3. 出现顺序越后，权重就越大。（导入样式表在前，主样式表在后）

**【第三章练习代码：<https://jsbin.com/cukacal/edit?html,css,output>】**

##第四章 值和单位##

###3.1数字###

有整数、实数（小数），且都可以是正数或负数。

###3.2百分数###

总是相对于另一个值。

###3.3颜色###

| 颜色名    | 百分数记法            | 三元组记法（数值）      | 十六进制颜色记法 | 简写记法  |
| ------ | ---------------- | -------------- | -------- | ----- |
| red    | rgb(100%,0%,0%)  | rgb(255,0,0)   | \#FF0000 | \#F00 |
| orange | rgb(100%,40%,0%) | rgb(255,102,0) | \#FF6600 | \#F60 |

- 颜色名：按名使用为数不多的几种颜色。现存的颜色名有：aqua、fuchsia、lime、olive、red、white、black、gray、maroon、orange、silver、yellow、blue、green、navy、purple、teal。


- 百分数记法：百分数值为0%～100%，可以有小数（但用户代理可能会忽略小数点），如果值落在可取范围之外，都会“裁剪”到最接近的范围边界。如，130%会默认调整为100%。
- 三元组记法（数值）：整数范围为0～255
- 十六进制颜色记法：将三个00～FF的十六进制连起来，就可以设置一种颜色，即\#RRGGBB。
- 简写记法：如果十六进制的3组数都是成对的，就可以采用简写记法，即\#RGB。
- web安全颜色：在256色计算机系统上避免抖动的颜色。通常为20%、51、\#33的倍数。

###3.4长度###

- 绝对长度

  | 绝对长度单位 | 缩写   | 解释        | 换算              |
  | ------ | ---- | --------- | --------------- |
  | 英寸     | in   | 美国尺子单位    | 1英寸=2.54厘米      |
  | 厘米     | cm   | 全世界尺子单位   | 1厘米=0.394英寸     |
  | 毫米     | mm   |           | 1毫米=0.1厘米       |
  | 点      | pt   | 标准的印刷度量单位 | 1英寸=72点         |
  | 派卡     | pc   | 印刷术语      | 1派卡=12点；1英寸=6派卡 |


- 相对长度

  | 相对长度单位 | 解释                                  | 补充说明                                     |
  | ------ | ----------------------------------- | ---------------------------------------- |
  | em     | em-height，1个em 定义为一种给定字体的font-size。 | 若是针对字体，em则相对父元素的font-size；若水真对非字体，em则相对于该元素的font-size。 |
  | ex     | x-height，常用的印刷度量单位。                 | 所用字体中小写x的高度。目前很多用户代理方便的假设1ex=0.5em。      |
  | px     | 像素，取决于显示设备的分辨率。                     | 若按像素设置字体大小，用户将无法使用“文本大小”菜单调整文本的大小，但像素非常适合来度量图像大小。 |

###3.5URL###

- 绝对URL

  url(protocol://server/pathname)， 无论这个URL放到哪里，它都能正常工作。

- 相对URL 

  url(pathname)，相对于该URL所在文档的位置。使用多层嵌套调用时，注意相对URL要相对于样式表本身。

###3.6关键字###

用某个词来描述的值，例如none，underline，normal等。inherit可以使一个属性的值与其父元素的值相同。

【第四章练习代码：<https://jsbin.com/sujazub/edit?html,css,output>】

##第五章 字体##

###5.1字体系列 font-family###

| 通用字体       | 特点          | 包括的指定字体                                  |
| ---------- | ----------- | ---------------------------------------- |
| Serif      | 成比例，有上下短线   | **Times**, Georgia, New Century Schoolbook |
| Sans-serif | 成比例，没有上下短线  | **Helvetica**, Geneva, **Verdana**, **Arial**, Univers。 |
| Monospace  | 不成比例，常用于打字机 | Courier, Courier New, Andale Mono        |
| Cursive    | 模仿手写体       | Zapf Chancery, Author, Comic Sans        |
| Fantasy    | 其他          | Western, Woodblock, Klingon              |

注意⚠️：使用以上有空格或带符号的字体时，要在字体外部加引号（单、双引号都可以），如`'New Century Schoolbook'`。但若字体名只包含一个词，就不能加引号。

通用字体：

如`font-family:sans-serif`，若选择某一种通用字体，用户代理就会从通用字体中选择一个具体指定字体（如Helvetica）。

指定字体：

如`font-family:Georgia;`，如果该字体不可用，用户代理会使用默认字体。因此建议在所有font-family中都提供一个通用字体系列，如`font-family: Arial, sans-serif`

###5.2字体加粗 font-weight###

字体有9级加粗度，分别为**100-900**。除此之外还有关键字设置：**normal | bold | bolder | lighter | inherit**。

经chrome测试，发现bold和bolder没有明显区别。字体粗细程度为lighter（100）->normal（400）->bold，bolder（600）。可嵌套使用，存在嵌套的逐级递增关系，例如bold内使用一层lighter则转为normal，使用两层ligher则转为ligher。

###5.3字体大小 font-size###

字体本身有一个em框（即忽略行间距的字体基线间的距离），font-size为给定字体的em框提供一个大小。font-size的值可以为以下几种：

有七个绝对大小值：**xx-small | x-small | small | medium | large | x-large | xx-large**，其中medium和用户代理默认的字体大小相同（chrome下），此7个值中不存在嵌套的逐级递增关系。缩放因子由用户代理决定。

两个相对大小值：**larger | smaller**，存在嵌套的逐级递增关系，且可以突破xx-small和xx-large的范围。缩放程度由用户代理决定，通常为1.2。

**百分数值**，存在嵌套的逐级递增关系。

**长度单位**，如pt, pc, in, cm, mm等。

**inherit**

###5.4字体风格 font-style###

可使用的值为 **italic | oblique  | normal | inherit**

italic 和 oblique 都是斜体，但是italic对每个字母的结构有一些小改动，而oblique是将正常竖直文本倾斜；chrome中测试了几种不同通用字体，目前没发现具体细节区别。

如果不存在 italic 字体,而存在oblique 字体,则使用后者 oblique 字体;如果不存在 oblique 字体,而存在italic 字体,则不能用 italic 替代,此时浏览器会将正常直立字体倾斜一个角度来作为倾斜字体使用;

###5.5字体变形 font-variant###

可使用的值为 **small-caps | normal | inherit **

small-caps :小型大写字母。当文本源的字母是小写时，就会显示一个小型的大写字母，当文本源的字母是大写时，就会显示一个更大的大写字母。

###5.6font属性 font###

将所有font内容都合并到一个属性中，作为涵盖所有其他字体属性的简写。格式为：**font-style || font-weight || font-variant, font-size / line-height, font-family**

属性值为norma可以忽略，前三个值font-style, font-weight, font-variant允许任意顺序，但font-size, font-family需作为最后两个值，必须指定，且顺序不可变。其中，font-size还可选择是否添加line-height。

例如`h1{font: italic 900 small-caps 30px Verdana, Helvetica, Arial, sans-serif;}`，添加line-height：`h1{font: bold italic 200%/1.2 Verdana, Helvetica, Arial, sans-serif;}`

注意⚠️：使用简写属性font时，所有被忽略的值都会重置为其默认值。因此注意当前font属性是否将之前的定义覆盖。

【第五章练习代码：<https://jsbin.com/wiyujuy/edit?html,output>】

##第六章 文本属性##

6.1缩进 text-indent 和水平对齐

一个段落的第一行缩进，可选值为**\<length> | \<percentage> | inherit **，若值为负数则呈现悬挂缩进的效果。

注意⚠️：其中的percentage是相对于其父元素的宽度。

6.2垂直对齐

6.3字间隔和字母间隔

6.4文本转换

6.5文本装饰

6.6文本阴影

##第七章 基本视觉格式化##

##第八章 内边距、边框和外边距##

##第九章 颜色和背景##

##第十章 浮动和定位##

##第十一章 表布局##

##第十二章 列表与生成内容##

##第十三章 用户界面样式##

##第十四章 非屏幕媒体##