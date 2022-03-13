[Toc]

# 1 创建项目

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

# 2 创建项目_移动端适配

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
		| --- MSite ----------------------------- 首页组件文件夹
			| --- MSite.vue ----------------------------- 首页组件 vue
		| --- Search ----------------------------- 搜索组件文件夹
			| --- Search.vue ----------------------------- 搜索组件 vue
		| --- Order ----------------------------- 订单组件文件夹
			| --- Order.vue ----------------------------- 订单组件 vue
		| --- Profile ----------------------------- 个人组件文件夹
			| --- Profile.vue ----------------------------- 个人组件 vue
			
	| --- App.vue ----------------------------- 应用根组件 vue
	| --- main.js ----------------------------- 应用入口
```

## 3.4 配置路由

1. 安装
   1. `npm i vue-router@3.5.3`(vue2.x只能搭配3.5.3版本)

2. 按照上面的组件结构创建目录

3. 创建router目录存放路由

   1. ```
      src
      	| --- router ----------------------------- 路由组件文件夹
      		| --- index.js ----------------------------- 向外暴露的路由器模块
      		| --- router.js ----------------------------- 存放并暴露路由数组
      ```

   2. index.js

      1. ```javascript
         /* 
             向外暴露路由器模块
         */
         
         import Vue from 'vue'
         import VueRouter from 'vue-router'
         // 引入路由组件
         import routes from './router'
         // 声明使用vue插件
         Vue.use(VueRouter)
         export default new VueRouter({
             mode: 'history', // 路由路径没有#
             routes,
         })
         ```

   3. router.js

      1. ```javascript
         /* 
             路由配置的数组
         */
         
         // 引入路由组件
         import MSite from '@/pages/MSite/MSite.vue'
         import Search from '@/pages/Search/Search.vue'
         import Order from '@/pages/Order/Order.vue'
         import Profile from '@/pages/Profile/Profile.vue'
         export default [
             { path: '/msite', component: MSite },
             { path: '/search', component: Search },
             { path: '/order', component: Order },
             { path: '/profile', component: Profile },
             { path: '/', redirect: '/msite' },
         ]
         ```

   4. App.vue测试路由

      1. main.js

         1. ```javascript
            import Vue from 'vue'
            import App from './App.vue'
            import 'lib-flexible'
            import router from './router'
            
            Vue.config.productionTip = false
            
            new Vue({
                render: (h) => h(App), // components和template的方法需要额外在vue.config.js上配置
                router, // 所有组件都能看到$router和$route <router-link> 和 <router-view>
            }).$mount('#app')
            ```

      2. App.vue

         1. ```vue
            <template>
                <div>
                    <router-view></router-view>
                    <FooterGuide></FooterGuide>
                </div>
            </template>
            
            <script>
                import FooterGuide from '@components/FooterGuide/FooterGuide.vue'
            
                export default {
                    name: 'App',
                    components: {
                        FooterGuide,
                    },
                }
            </script>
            ```

4. 测试路由

   1. 在`http://localhost:8080/msite`中测试路由`/profile`,`/order`,`search`



# 4 footerGuide组件

## 4.1 `src/assets/stylus/mixin.styl`文件

```stylus
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

## 4.2 FooterGuide组件

```vue
<template>
    <div class="footer-guide">
        <span class="guide-item" :class="{on: $route.path==='/msite'}" @click="goto('/msite')">
            <span>
                <i class="iconfont icon-home"></i>
            </span>
            <span>首页</span>
        </span>
        <span class="guide-item" :class="{on: $route.path==='/search'}" @click="goto('/search')">
            <span>
                <i class="iconfont icon-search"></i>
            </span>
            <span>搜索</span>
        </span>
        <span class="guide-item" :class="{on: $route.path==='/order'}" @click="goto('/order')">
            <span>
                <i class="iconfont icon-order"></i>
            </span>
            <span>订单</span>
        </span>
        <span class="guide-item" :class="{on: $route.path==='/profile'}" @click="goto('/profile')">
            <span>
                <i class="iconfont icon-account"></i>
            </span>
            <span>我的</span>
        </span>
    </div>
</template>

<script type="text/ecmascrspanpt-6">
    export default {
        name: 'FooterGuide',
        data() {
            return {

            }
        },
        methods:{
            goto(path){
                this.$router.replace(path)
            }
        },
        components: {

        }
    }
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
    // 没有路径提示, 包括老师写的都没有
    @import '../../assets/stylus/mixins.styl'
    .footer-guide
        top-border-1px(#cccccc)
        position  fixed
        height 50px
        left: 0
        bottom 0
        width 100%
        display flex
        align-items  center
        justify-content  space-between
        .guide-item
            display: flex
            flex-direction: column
            width: 25%
            text-align: center
            &.on
                color $green
            span
                margin-top: 3px
                font-size 12px
                i
                    font-size 22px

</style>

```

## 4.3 index.html引入iconfont项目链接

- 在iconfont网站添加图标到项目,然后生成链接引入到html文件中

- ```html
  <link rel="stylesheet" href="http://at.alicdn.com/t/font_3239724_54jjozw3nd6.css">
  ```

## 4.4 vue文件中@import没有路径提示

- 尚待解决, 视频里老师也没有路径提示



# 5 使用git管理项目

## 5.1 基于本地项目创建git仓库

1. `git init`创建仓库
2. `git add.`添加到暂存区
3. `git commit -m ''`提交到版本区
4. `git remote add origin [远程仓库地址]`设置远程仓库
5. `git push origin master`推送到远程仓库 // 虽然政治正确更改为了main,但是命令还是master // 3.11更新:好像改回来了
6. `git branch`查看本地分支
7. `git checkout -b my`创建新分支my并跳转到my分支
8. `git push origin my`推送到远程my分支(后面开发都在my分支开发)
9. `git checkout master`切换到master分支,先切换才能合并
10. `git merge my`合并my分支到master
11. `git push origin master`再次推送到远程master分支
12. `git pull origin my`用于其他开发者修改代码后从远程仓库下载到本地
13. `git clone [仓库地址]`下载远程仓库文件到本地,下载所有分支到本地, 但是只有master分支默认存在(`git branch`只显示master)
    1. `git checkout -b my origin/my`创建一个my分支对应远程仓库的my分支
    2. 瞬间完成,不需要额外下载代码

# 6 导航路由组件_静态



# 7 Header组件_props与slot

## 7.1 Header组件要求

- ```
  整体
  	背景色 #02a774
  	位置固定顶部
  	宽 100%
  	高 45px
  左
  	距整体左边界15px
  	垂直居中
  	宽 10%
  	高 50%
  中
  	容器水平垂直居中
  	宽 50%
  	字体颜色 #fff
  	容器内部字体水平居中
  ```

## 7.2 Header组件的编写

<img src="C:\Users\董磊\AppData\Roaming\Typora\typora-user-images\image-20220313210800728.png" alt="image-20220313210800728" style="zoom:200%;" />

1. 因为多个组件使用, 所以不再一个一个引入了, 直接在main.js中引入作为全局组件

```js
/* 注册全局组件 */
import Header from '@components/Header/Header.vue'
Vue.component('Header', Header)
```

2. 在MSite.vue, Order.vue ,Pprofile.vue, Search.vue等组件中直接使用`<Header>`组件

```html
        <Header :title="'正在定位中'">
            <span slot="left" class="header_search">
                <i class="iconfont icon-search"></i>
            </span>
            <span slot="right" class="header_login">
                <span class="header_login_text">登录 | 注册</span>
            </span>
        </Header>
```

```vue
        <Header title="订单"></Header>
```

```vue
        <Header title="个人中心"></Header>
```

```vue
        <Header title="搜索"></Header>
```

3. 问题: vue-router 连续点击同一路由报错(路由冗余), 在footerGuide.vue中修改goto方法如下

```js
        goto (path) {
            this.$router.replace(path).catch(err => { })
        }
```

