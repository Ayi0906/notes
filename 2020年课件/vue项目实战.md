[Toc]

# 1.创建项目

- `vue create [projectName]` // 这里创建的是vue2.x的项目

- ```json
  // package.json
  {
      "name": "project2",
      "version": "0.1.0",
      "private": true,
      "scripts": {
          "serve": "vue-cli-service serve",
          "build": "vue-cli-service build",
          "lint": "vue-cli-service lint"
      },
      "dependencies": {
          "babel-plugin-component": "^1.1.1",
          "core-js": "^3.6.5",
          "lib-flexible": "^0.3.2",
          "postcss-px2rem": "^0.3.0",
          "stylus": "^0.56.0",
          "stylus-loader": "^3.0.2",
          "vue": "^2.6.11"
      },
      "devDependencies": {
          "@vue/cli-plugin-babel": "~4.5.15",
          "@vue/cli-plugin-eslint": "~4.5.15",
          "@vue/cli-service": "~4.5.15",
          "babel-eslint": "^10.1.0",
          "eslint": "^6.7.2",
          "eslint-plugin-vue": "^6.2.2",
          "vue-template-compiler": "^2.6.11"
      },
      "eslintConfig": {
          "root": true,
          "env": {
              "node": true
          },
          "extends": [
              "plugin:vue/essential",
              "eslint:recommended"
          ],
          "parserOptions": {
              "parser": "babel-eslint"
          },
          "rules": {
              "no-unused-vars": 0
          }
      },
      "browserslist": [
          "> 1%",
          "last 2 versions",
          "not dead"
      ]
  }
  ```

- 

## 1.1 配置jsconfig.json

- 新建一个`jsconfig.json`文件

  - 目录中存在`jsconfig.json`文件时，表明该目录是 JavaScript 项目的根目录。`jsconfig.json`文件指定了根文件以及 [JavaScript 语言服务](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fmicrosoft%2FTypeScript%2Fwiki%2FJavaScript-Language-Service-in-Visual-Studio) 提供的功能选项

  - ```json
    {
        "compilerOptions": {
            "target": "ES6", // 指定要使用的默认库
            "module": "commonjs", // 指定模块系统（生成模块代码时）
            "allowSyntheticDefaultImports": true, // 允许从模块进行默认导入而没有默认导出。这不影响代码生成，仅影响类型检查
            "baseUrl": "./", // 基本目录，用于解析非相对模块名称
            "paths": {
                // 指定要相对于 baseUrl 选项计算的路径映射
                "@/*": ["src/*"],
                "@components/*": ["src/components/*"]
            }
        },
        "exclude": ["node_modules", "dist"] // 告诉语言服务哪些文件不属于源代码
    }
    ```

## 1.2 配置prettier.config.json文件

- perttier.config.json决定perttier格式化规则

- 在vscode中直接在插件商城下载, 其它时候需要去npm下载并重新编写格式化规则

- 新建`prettier.config.json`并配置代码

  - ```json
    module.exports = {
        tabWidth: 4, // 指定每个缩进级别的空格数。 // 默认2
        useTabs: false, // 用制表符而不是空格缩进行。// 默认false // 制表符将用于缩进，但Prettier使用空格来对齐事物，例如在三元音中
        semi: false, //在语句末尾打印分号 // 仅在可能引入 ASI 故障的行首添加分号
        singleQuote: true, // 使用单引号而不是双引号。
        trailingComma: 'es5', // 尾随逗号在ES5中有效
        bracketSpacing: true, //在对象文本中的括号之间打印空格。
        bracketSameLine: true, // 将多行 HTML（HTML、JSX、Vue、Angular）元素放在最后一行的末尾，而不是单独放在下一行（不适用于自闭合元素）。
        arrowParens: 'always', //箭头函数始终包含括号
        requirePragma: false,
        vueIndentScriptAndStyle: true, //缩进 Vue 文件中的脚本和样式标签。
    }
    ```

## 1.3 package.json文件的配置

- 绝大多数的配置已经配置完毕,添加几条eslint的配置,消掉有些情况下eslint在开发时不允许代码内有console代码的报错

  - ```json
    "eslintConfig": {
        // 省略其它配置
        "rules": {
            "no-console": 0, // 取消掉有些情况下eslint在开发时不允许代码内有console代码的报错
            "no-mixed-spaces-and-tabs": 0
        }
    },
    ```

# 2.创建项目_移动端适配

## 2.1 安装`postcss-px2rem` 和`lib-flexible`

- 安装`postcss-px2rem` 和`lib-flexible`
  - `npm i postcss-px2rem lib-flexible`

## 2.2 配置`vue.config.js`和`main.js`

- 配置`vue.config.js`

  - @vue/cli3以上需要自行创建`vue.config.json`文件

  - 配置代码

    - ```javascript
      const { resolve } = require('path')
      const px2rem = require('postcss-px2rem')
      const postcss = px2rem({
          // 基准大小, baseSize需要和rem.js中单位rem值占比一样
          remUnit: 37.5, // 设计稿等分之后的值, 等分的比例同页面rem的比例是一致的, 如果设计稿750, 写75
      })
      
      module.exports = {
          // 内部写webpack原生配置
          configureWebpack: {
              resolve: {
                  extensions: ['.js', '.vue', '.json'], // 可以省略的后缀名
                  alias: {
                      // 路径别名
                      // vue$: 'vue/dist/vue.esm.js', // 表示精准匹配 带vue编辑器
                      '@': resolve(__dirname, './src'), // 路径别名
                      '@components': resolve(__dirname, './src/components'),
                  },
              },
          },
          css: {
              // 添加postcss配置
              loaderOptions: {
                  postcss: {
                      plugins: [postcss],
                  },
              },
          },
      }

- 在main.js中引入`lib-flexible'`

  - ```javascript
    import 'lib-flexible'
    ```

- 在index.html中添加元信息

  - ```html
    <meta
    	name="viewport"
    	content="width=device-width,initial-scale=1.0,maximum-scale=1.0,minimum=1.0,user-scalable=no"
    />
    ```

## 2.3 解决点击响应0.3秒延迟

- 代码

- ```javascript
  <!-- <script src="https://cdn.bootcdn.net/ajax/libs/fastclick/1.0.6/fastclick.js"></script> // 这是非压缩代码 -->
  <script src="https://cdn.bootcdn.net/ajax/libs/fastclick/1.0.6/fastclick.min.js"></script>
  
  <script>
      if ('addEventListener' in document) {
          document.addEventListener(
              'DOMContentLoaded',
              function () {
                  FastClick.attach(document.body)
              },
              false
          )
      }
      /* // jquery使用方法
              $(function () {
                  FastClick.attach(document.body)
              }) */
  </script>
  ```

- 说明

  - 自行前往bootcdn查找fastclick链接
    - `https://www.bootcdn.cn/fastclick/`
  - 前往github查找它的相应代码
    - `https://github.com/ftlabs/fastclick`

- `[5步解决移动设备上的300ms点击延迟 - Van小时 - 博客园 (cnblogs.com)](https://www.cnblogs.com/vanstrict/p/5700957.html)`

# 3.搭建整体路由界面

## 3.1 项目源码目录设计

1. `src/api`:与后台交互模块文件夹
2. `src/pages`:路由组件文件夹
3. `src/components`:非路由组件文件夹 
4. `src/router`:路由器组件文件夹
5. `src/common或者src/assets`:通用资源文件夹 
   1. `src/common/fonts`
   2. `src/common/img`
   3. `src/common/stylus`
   4. 等等
6. `src/store`:vuex相关模块文件夹
7. `src/filters`:自定义过滤器模块文件夹
8. `src/mock`:模拟数据接口文件夹
9. `App.vue`:应用组件
10. `main.js`:入口js

## 3.2 搭建stylus环境

1. 安装

   1. `npm i stylus stylus-loader@3.0.2` // 最新版6.0.2适配vue2.x报错

2. 创建`src/common/stylus`文件夹

3. 编写一个`src/common/stylus/mixins.styl`文件

   1. ```stylus
      $green = #02a774
      $yellow = #f5a100
      $bgc = #e4e4e4
      // 一像素边框
      bottom-border-1px($color)
          position relative
          border none
          &::after
              content ''
              position absolute
              left 0
              bottom 0
              width 100%
              height 1px
              background-color $color
              transform scaleY(0.5)
      // 一像素上边框
      top-border-1px($color)
          position relative
          &::before
              content ''
              position absolute
              z-index 200
              left 0
              top 0
              width 100%
              height 1px
              background-color $color
      // 根据像素比缩放1px像素边框
      @media only screen and (-webkit-device-pixel-ratio 2)
          .border-1px
              &::before
                  transform scaleY(0.5)
      @media only screen and (-webkit-device-pixel-ratio 3)
          .border-1px
              &::before
                  transform scaleY(0.333333)
      // 根据像素比来实现 2X图 3X图
      bg-image($url)
          background-image url($url + '@2x.png')
          @media (-webkit-device-pixel-ratio 3), (min-device-pixel-ratio 3)
              background-image url($url + '@3x.png')
      // 清除浮动
      clearFix()
          &::after
              content ''
              display block
              clear both
      ```

4. .styl文件的编写

   1. 在.vue文件中

      1. ```vue
         <style lang="stylus" rel="stylesheet/stylus" scoped></style>
         ```

   2. 引入其它.styl文件

      1. ```html
         <style lang="stylus" rel="stylesheet/stylus" scoped>
             @import './common/stylus/mixins.styl'
         </style>
         ```

## 3.3 分析应用的整体vue组件结构

```
src
	| --- components ----------------------------- 非路由组件文件夹
		| --- FooterGuide ----------------------------- 底部组件文件夹
			| --- FootGuide.vue ----------------------------- 底部组件 vue
	| --- pages ----------------------------- 路由组件文件夹
		| --- Msite ----------------------------- 首页组件文件夹
			| --- Msite.vue ----------------------------- 首页组件 vue
		| --- Search ----------------------------- 搜索组件文件夹
			| --- Search.vue ----------------------------- 搜索组件 vue
		| --- Order ----------------------------- 订单组件文件夹
			| --- Order.vue ----------------------------- 订单组件 vue
		| --- Profile ----------------------------- 个人组件文件夹
			| --- Profile.vue ----------------------------- 个人组件 vue
			
	| --- App.vue ----------------------------- 应用根组件 vue
	| --- main.js ----------------------------- 应用入口
```

