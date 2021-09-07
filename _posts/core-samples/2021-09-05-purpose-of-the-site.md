---
layout: post
title: "本站之初衷"
tagline: ""
description: "本站之初衷"
category: Jekyll
tags: []
last_updated: 2021-09-07
---

本站之初衷为记录我于电脑之使用，记录成篇，篇成站。
+ 之所以记录，为能用故也
  + 便于**查询**，于是乎必有搜索，必有云服务
  + 便于**整理**，于是乎必有标签，目录
    - 目录，硬件，操作系统，应用软件，学术
	- 标签，关键词
  + 便于**转移**，必可用于多OS，多编程语言，于是乎求简，求常用。
 
## 篇之格式，适应墨水屏
  + 发布时间、修改时间、修改记录
  + 标题分级
  + 图文对齐
  + 字，字体、加粗、颜色分深浅
  + 行内引用，段落引用
    1. 代码块，自动缩进，自动关键词变色，色彩方案可调整 
	2. URL等路径
	3. 操作流程
  + 超链接
  + 参考文献

## 方案选择

+ 备选方案，Windows OS\+word之doc文件\+网盘
  + 难以查询，比如手机、网吧
+ 备选方案，笔记服务如Onenote，有道云笔记。
  + 不可深度定制功能，不可增有用而去多余
  + 一旦更新，只能被动适应，徒增成本
  + 服务存废不由己
  
是以选定Github Page\+Markdown格式。



## Emphasis

**This is bold text**

__This is bold text__

*This is italic text*

_This is italic text_

<s>Strikethrough</s>


## Blockquotes

Blockquotes with plain text

> Blockquotes Text

Blockquotes with Lists

> - List one
> 	- List one.one
>   - List one.two
> - List two
> - List three

To end the Blockquotes just put an empty line under \>

## Lists

Unordered

+ Create a list by starting a line with `+`, `-`, or `*`
+ Sub-lists are made by indenting 2 spaces:
  - Marker character change forces new list start:
    * Ac tristique libero volutpat at
    + Facilisis in pretium nisl aliquet
    - Nulla volutpat aliquam velit
+ Very easy!

Ordered

1. Lorem ipsum dolor sit amet

	This is ordered List one content

2. Consectetur adipiscing elit
3. Integer molestie lorem at massa

This is another ordered list

1. You can use sequential numbers...
1. ...or keep all the numbers as `1.`

Start numbering with offset:

57. foo
1. bar


## Code

Inline `code`

Indented code

    // Some comments
    line 1 of code
    line 2 of code
    line 3 of code


Block code "fences"

```
Sample text here...This is code block...paste some code here to try
```


## Tables

| Option | Description |
| ------ | ----------- |
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |

Right aligned columns

| Option | Description |
| ------:| -----------:|
| data   | path to data files to supply the data that will be passed into templates. |
| engine | engine to be used for processing templates. Handlebars is the default. |
| ext    | extension to be used for dest files. |


## Links

markdown syntax

	[link text](http://einverne.github.com)

output:

[link text](http://einverne.github.com)

Add "title text" (which shows up under the cursor)

	[link with title](http://einverne.github.io/ "title text!")

output:

[link with title](http://einverne.github.io/ "title text!")

Most URLs will automatically be turned into links. To be explicit, just write it like this:

	Autoconverted link <https://github.com/einverne>

output:

Autoconverted link <https://github.com/einverne>

## Reference Links

	You can also put the [link URL][1] below the current paragraph
	like [this][2].

	   [1]: http://url
	   [2]: http://another.url "A funky title"

Output:

You can also put the [link URL][1] below the current paragraph
like [this][2].

   [1]: http://url
   [2]: http://another.url "A funky title"

Here the text "link URL" gets linked to "http://url", and the lines showing "[1]: http://url" won't show anything.

Or you can use a [shortcut][] reference, which links the text "shortcut" to the link named "[shortcut]" on the next paragraph.

	Or you can use a [shortcut][] reference, which links the text
	"shortcut" to the link named "[shortcut]" on the next paragraph.

	[shortcut]: http://goes/with/the/link/name/text

Output:

Or you can use a [shortcut][] reference, which links the text "shortcut" to the link named "[shortcut]" on the next paragraph.

[shortcut]: http://goes/with/the/link/name/text

## Images

To include an image, just put a "!" in front of a text link:

	![Minion](https://octodex.github.com/images/minion.png)

output:
![Minion](https://octodex.github.com/images/minion.png)

The alternate text will show up if the brower can't load the image.
You can also use a title if you want, like this:

	![Stormtroopocat](https://octodex.github.com/images/stormtroopocat.jpg "The Stormtroopocat")

output:

![Stormtroopocat](https://octodex.github.com/images/stormtroopocat.jpg "The Stormtroopocat")

Like links, Images also have a footnote style syntax

	![Alt text][id]
	[id]: https://octodex.github.com/images/dojocat.jpg  "The Dojocat"

output:

![Alt text][id]

With a reference later in the document defining the URL location:

[id]: https://octodex.github.com/images/dojocat.jpg  "The Dojocat"

## emoji

+1 :+1:  
smile :smile:  
laughing :laughing:  
wink :wink:  
grin :grin:  
cry :cry:  
confused :confused:  
yum :yum:  

You can find the Emoji from this [link](http://www.emoji-cheat-sheet.com/)

## Footnotes

Footnote 1 link[^first].

Footnote 2 link[^second].

Inline footnote^[Text of inline footnote] definition.

Duplicated footnote reference[^second].

[^first]: Footnote **can have markup** and multiple paragraphs.

	paragraph 2 this is _some text_。

[^second]: Footnote text.


## Abbreviations

This is HTML abbreviation example.

It converts "HTML", but keep intact partial entries like "xxxHTMLyyy" and so on.

*[HTML]: Hyper Text Markup Language

因为本 Jekyll 在 `_config.yml` 中配置使用 `kramdown` markdown解释器，所以更多的语法可以参考官方语法[页面](http://kramdown.gettalong.org/syntax.html)


## 公式

使用 MathJax

使用行内模式 `$x^2$` 显示为 $x^2$

使用块模式，用 `$$ ... 公式内容 ... $$` 来格式：

$$
\frac{x1*5+x2*4+x3*3+x4*2+x5}{5}*2
$$

$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}$$


这里提供在线 LeTex 公式编辑

- <https://www.codecogs.com/latex/eqneditor.php>
