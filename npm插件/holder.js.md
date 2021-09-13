- 安装

  - npm i holderjs -D

- 使用

  - 引入html文件

    - `<script src="holder.js"></script>`

  - 填充图片

    - `<img src="holder.js/300x200">`
    - ![image-20210912202509124](C:\Users\董磊\AppData\Roaming\Typora\typora-user-images\image-20210912202509124.png)

  - 为了避免控制台404错误，可以使用data-src而不是src。

    - `<img data-src="holder.js/100x100">`

  - 在指定区域使用

    - ```
          <div id="test"></div>
          <script>
              Holder.addTheme("new", {
                  fg: "red", // 字体颜色
                  bg: "orange", // 背景颜色
                  size: 10
              }).addImage("holder.js/200x200?theme=new", "#test").run()
          </script>
      ```

  - 使用百分比为单位

    - ```
          <style>
              img {
                  width: 100%;
                  height: 250px;
                  border: 1px solid black;
              }
          </style>
      </head>
      
      <body>
          <img src="" alt="" data-src="holder.js/50px200">
      </body>
      ```

    - `50px200`中的p代表百分比符号

  - theme:设定holderjs主题配色:sky,vine,lava,gray,industrial,social

    - `<img src="" alt="" data-src="holder.js/200x200?theme=sky">`

  - random:设定随机主题色

    - `<img src="" alt="" data-src="holder.js/200x200?random=yes">`

  - outline:画出图片轮廓和对角线的占位符

    -  `<img src="" alt="" data-src="holder.js/200x200?outline=yes">`

  - 参数

    - bg:背景颜色
    - fg:字体颜色
    - text:文本,如果需要换行加上` \n `,注意` \n `的两边有空格
      - `<img src="" alt="" data-src="holder.js/200x200?text=hello \n world">`

  - 自定义图片主题

    - ```
          <img src="" alt="" data-src="holder.js/200x200?theme=diy">
          <script>
              Holder.addTheme('diy', {
                  fg: 'red', // 字体颜色
                  bg: 'yellow', // 背景颜色
                  size: 20, // 字体大小
                  font: 'Monospace', // 字体样式
                  fontweight: "200", // 字体粗细
                  text: 'hello world' // 文本
              })
          </script>
      ```

      