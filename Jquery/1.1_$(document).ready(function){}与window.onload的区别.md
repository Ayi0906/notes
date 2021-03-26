## 网页加载的顺序
1 域名解析：比如www.baidu.com

2 加载HTML，加载百度首页的HTML代码

3 加载脚本和样式：加载百度的CSS和JS代码或者其它外部脚本

4 解析并执行脚本文档代码：执行JS和CSS或其它外部脚本代码

5 构造HTML的DOM模型：构造百度首页的HTML的DOM

6 加载DOM元素

7 加载图片等非文字媒体文件

8 网页加载完毕

![](https://img-blog.csdnimg.cn/20190708101506910.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2RhcG9uaQ==,size_16,color_FFFFFF,t_70)

## 执行时机
- window.onload必须等待网页中的内容加载完毕后（包括图片），才能执行
- $(document).ready(function(){})在网页中所有DOM结构绘制完毕后就执行，可能DOM元素关联的东西并没有加载完。

>  `window.onload`必须等到页面内包括图片的所有元素和资源加载完毕后才能执行，也就是上述图片的时间点2

> `$(document).ready()`是DOM加载完毕后就执行，不必等到整个网页资源加载完毕，也就是在上述图片的时间点1。

## 编写个数不同
- window.onload只能编写一个
- $(document).ready(function(){})可以编写多个

## 简化写法
- window.onload没有简化写法
- $(document).ready(function(){})的简化写法是`$(function(){})`

