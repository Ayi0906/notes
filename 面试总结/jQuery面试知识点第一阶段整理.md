[Toc]
### 简述jq的 $(document).ready(function(){}) 与 window.onload的区别
- 网页加载的顺序
    1. 域名解析：比如www.baidu.com
    2. 加载HTML，加载百度首页的HTML代码
    3. 加载脚本和样式：加载百度的CSS和JS代码或者其它外部脚本
    4. 解析并执行脚本文档代码：执行JS和CSS或其它外部脚本代码
    5. 构造HTML的DOM模型：构造百度首页的HTML的DOM
    6. 加载DOM元素
    7. 加载图片等非文字媒体文件
    8. 网页加载完毕


- 执行时机

    - $(document).ready(function(){})
        - 是在网页中所有DOM结构都加载完毕，图片等媒体资源加载之前调用。（也就是第7步结束之后，第8步开始之前）
    - window.onload
        - 是在网页中的所有资源加载完成后才进行调用。（也就是第8步之后）

- 执行次数

    - $(document).ready(function(){})可以调用很多次。
    - window.onload只能调用一次。

- 简写

    - $(document).ready(function(){}) 可以简写为 $(fuction(){})