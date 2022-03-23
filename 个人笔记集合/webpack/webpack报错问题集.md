[Toc]

## 1 [webpack-cli] Unable to load '@webpack-cli/serve' command

- ```
  [webpack-cli] Unable to load '@webpack-cli/serve' command
  [webpack-cli] TypeError: options.forEach is not a function
  ```

- 报错详情

  - `npm -v`与`node -v`命令无误

  - package.json

    - ```json
              "webpack": "5.50.0",
              "webpack-cli": "^4.7.2",
              "webpack-dev-server": "4.0.0"
      ```

    - 本地安装以上三个包没有问题

    - 全局没有安装webpack
  
- 博客园上大多数都是重装webpack-cli,一点用都没有

- 在vscode的上输入`webpack -v`显示`'webpack' 不是内部或外部命令，也不是可运行的程序或批处理文件。`

- 我在全局环境下安装了webpack, webpack-cli ,调用`webpack -v`命令弹出版本

- 将webpack-cli升级至最新版解决了???????但是我的webpack-cli版本是在github的webpack-dev-server中抄的啊!!!!



## 2 The following asset(s) exceed the recommended size limit (244 KiB).

- 入口大小超过了建议的限制

- 解决:在配置中添加

  - ```js
    performance: { hints: false }
    ```

# 3 设置browserlist报错

> Make sure that your 'browserslist' includes only platforms that support these features or select an appropriate 'target' to allow selecting a chunk format by default. Alternatively specify the 'output.chunkFormat' directly.

- 我在browserslist的github文档中抄了以下代码

  - ```json
      "browserslist": [
        "defaults",
        "not IE 11",
        "maintained node versions"
      ]
    ```

- 解决: 删掉"maintained node versions"
