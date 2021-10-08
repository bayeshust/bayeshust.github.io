---
layout: post
tagline: ""
title: "网络视频获取"
description: "从视频网站下载视频"
category: 
tags: []
last_updated: 2021-10-08
---
犹记得当初视频获取之便利。今日互联网，能看到视频但不能下载。
提供视频解析服务的网站/软件

[硕鼠](https://www.flycd.com)

[b站（网页版）](https://bilibili.iiilab.com)

IDM、迅雷等下载工具捕获

# HLS（HTTP Live Streaming）协议视频获取

HLS把媒体分成一个.m3u8列表，和列表里的.ts分片视频[^1st]。获取原媒体只需要通过URL获取.m3u8文件即可。

## .m3u8下载地址之获取[^2nd]

### 一、使用Web浏览器

以央视影音的一篇视频为例，页面地址：<http://app.cctv.com/special/cbox/detail/index.html?guid=e8b80233f53c4b8fb35a7bd5f0e0b535&vsid=C10437#0>

使用Chrome浏览器打开这个网页，然后鼠标右键 -> inspect（检查），可以看到如下的界面：

![1](images/20210929-1.png)

注意切换到红色箭头所指的Network（网络请求）标签。

然后按Ctrl+R或浏览器刷新按钮，刷新网页，重新加载网页数据，这时这边的检查器的Network标签就会记录所有的网络文件请求，如下图：

![2](images/20210929-2.png)

待视频播放几秒钟后，可以点击红色箭头所指的按钮，停止录制。

![3](images/20210929-3.png)

然后点击上图中的搜索按钮（红色箭头所指）；然后搜索框中输入“.m3u8”来查找m3u8文件。
找到对应的文件，然后右键复制地址

![4](images/20210929-4.png)

找到m3u8文件后，鼠标右键 -> Copy（复制）-> Copy link address（复制链接地址）。

这样就成功获得m3u8的播放地址了。类似的Firefox可以用开发者工具（Web Develope Tool）。

### 二、Video Download Helper

Video Download Helper是两大Web浏览器Chrome、Firefox都有的扩展（extension/add-on）。安装好之后然后在视频页面直接点击插件图标，然后选择一个流，就可以获取到m3u8的地址。

![5](images/20210929-5.png)

## 得到目标视频

### 一、VLC播放器

我本来是用Potplayer，但是今天接触到VLC后有心用它取代Potplayer。多操作系统支持，功能强大，就是体积略大（40MB）。

![6](images/20210929-6.png)

按Ctrl+N，把刚才获取到的m3u8地址粘贴到地址栏中，如下图红色箭头处：

![7](images/20210929-7.png)

然后，注意蓝色箭头处，点击右侧的小三角，然后选择“转换”

![8](images/20210929-8.png)

将配置文件设置为mp4格式，然后目标文件设定为你想要保存的目录及文件名，然后点击开始，视频就开始转换啦。

### 二、Video Download Helper

Video Download Helper需要额外从Github上下载[支持程序](https://github.com/mi-g/vdhcoapp/releases/download/v1.6.3/VdhCoAppSetup-1.6.3.exe)。作者表示使用ffmpeg合并.ts文件。ffmepg中对h264的编码也使用了VLC所用x264来编码（Encode）[^3rd]，但是我做了一个测试，两种方法结果不同。Video Download Helper（下图左）生成质量不如VLC。

![9](images/20210929-9.png)

[^1st]: <https://en.wikipedia.org/wiki/HTTP_Live_Streaming>

[^2nd]: <https://www.jianshu.com/p/ce134cbd509c>

[^3rd]: <https://www.zhihu.com/question/48688851>








