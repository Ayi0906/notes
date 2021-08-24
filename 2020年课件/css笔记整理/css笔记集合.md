[Toc]

## 阿里iconfont图标的使用

- 下载
  - 去官网，按需下载
- 使用
  - 解压下载后的压缩包
  - 除了demo.index与demo.css文件无用，只是用作说明外。其它几个文件要放在一起。
  - 在html中引入iconfont.css文件
  - 通过`<span class="iconfont icon-direction-right"></span>`加载图标。其中iconfont类名为每一个图标都要有的类名。

- 如何修改iconfont图标的颜色和大小

  - 通过类名，id，或者span::before选中图标（直接选中span的tagName无效）

  - 修改font-size大小改变图标大小

  - 修改color改变图标颜色

  - ```
            span::before {
                font-size: 50px;
                color: red;
            }
    ```

