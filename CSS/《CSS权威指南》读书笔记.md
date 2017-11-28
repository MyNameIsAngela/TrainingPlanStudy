#《CSS权威指南》读书笔记#

**【2017-11-19 】 **

##第一章 CSS和文档##

###1.CSS和HTML的结合###

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

###1.通配选择器（*）、元素选择器（p）、类选择器（.）、id选择器（#）###

```html
//例子
* {... }
p.warning{...}
.warning.urgent{...}//两个类选择器链接在一起，仅可以选择同时包含这些类名的元素（类名的顺序不限）
```

###2.属性选择器###

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

###3.文档结构###

- 后代选择器

  `h1 em {color:gray;}`选择作为h1元素后代的任何em元素

- 选择子元素

  `h1 > strong {color:red;}`选择只作为一个h1元素子元素（而不是后代元素）的strong元素。

- 选择相邻兄弟元素

  `h1 + p {margin-top: 0;}`选择紧接在一个h1元素后出现的所有段落，h1要与p元素有共同的父元素。

###4.伪类和伪元素###

练习代码： <https://jsbin.com/tiqaduz/edit?html,css,output>

##第三章 结构和层叠##

##第四章 值和单位##

##第五章 字体##

##第六章 文本属性##

##第七章 基本视觉格式化##

##第八章 内边距、边框和外边距##

##第九章 颜色和背景##

##第十章 浮动和定位##

##第十一章 表布局##

##第十二章 列表与生成内容##

##第十三章 用户界面样式##

##第十四章 非屏幕媒体##