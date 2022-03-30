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

- [别再使用fastclick了 - 掘金 (juejin.cn)](https://juejin.cn/post/6844904168092614670)

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

# 8 login组件_meta

## 8.1 完成login组件的静态页面

## 8.2 使用路由元信息使<FooterGuide>根据路由的不同而显示

1. 逻辑

   1. 只有在/profile, /msite,/order,/search 等界面才显示<footerGuide>, 在/login路由不显示

2. meta

   1. 在路由定义文件中, 除path,component外添加meta对象

      1. ```js
             {
                 path: '/msite',
                 component: MSite,
                 meta: {
                     isShowFooter: true,
                 },
             },
         ```

   2. 在vue组件中通过`$route.meta.isShowFooter`来访问自定义的数据

      1. ```vue
                 <FooterGuide v-show="$route.meta.isShowFooter"></FooterGuide>
         ```

# 9 使用Swiper实现静态组件轮播

- 视频里下载的是Swiper@5.2.1
- https://swiper.com.cn/api/index.html 去这个网站的页面里找swiper5的用法
- https://swiper.com.cn/usage/index.html 找到html结构

## 9.1 下载swiper@5.2.1

- `npm i swiper@5.2.1 -S`

- 在.vue文件中引入swiper.js和swiper.css文件

  - ```js
    // 引入swiper主文件
    import Swiper from 'swiper'
    ```

  - ```html
    <style lang="css" scoped>
    @import '../../../node_modules/swiper/css/swiper.css';
    </style>
    <style lang="scss" scoped>
    @import '@/assets/scss/Msite.scss';
    </style>
    ```

  - 注意swiper.css要引入在所有样式文件的最前面

- 在mounted里面写入代码, 因为只有mounted才能获取到元素

  - ```js
    	mounted () {
    		// swiper对象必须在列表数据显示之后创建
    		new Swiper('.msite_nav_swiper-container', {
    			// direction: 'vertical', // 垂直切换选项
    			loop: true, // 循环模式选项
    
    			// 如果需要分页器
    			pagination: {
    				el: '.swiper-pagination',
    			}
    		})
    		new Swiper('.shop_list_container', {
    			direction: 'vertical',
    			slidesPerView: 'auto',
    			freeMode: true,
    			scrollbar: {
    				el: '.swiper-scrollbar',
    			},
    			mousewheel: true,
    		})
    	},
    ```

  - 这里用了两种swiper: 一种是传统横向轮播图, 还有一种是竖向内容查看轮播图

    - 横向轮播图

      - ```html
        <div class="swiper">
            <div class="swiper-wrapper">
                <div class="swiper-slide">Slide 1</div>
                <div class="swiper-slide">Slide 2</div>
                <div class="swiper-slide">Slide 3</div>
            </div>
            <!-- 如果需要分页器 -->
            <div class="swiper-pagination"></div>
            
            <!-- 如果需要导航按钮 -->
            <div class="swiper-button-prev"></div>
            <div class="swiper-button-next"></div>
            
            <!-- 如果需要滚动条 -->
            <div class="swiper-scrollbar"></div>
        </div>
        导航等组件可以放在Swiper容器之外
        ```

      - 允许给轮播图设置大小

      - ```css
        .swiper {
            width: 600px;
            height: 300px;
        }  
        ```

      - 配置

      - ```js
          var mySwiper = new Swiper ('.swiper', {
            direction: 'vertical', // 垂直切换选项
            loop: true, // 循环模式选项
            
            // 如果需要分页器
            pagination: {
              el: '.swiper-pagination',
            },
            
            // 如果需要前进后退按钮
            navigation: {
              nextEl: '.swiper-button-next',
              prevEl: '.swiper-button-prev',
            },
            
            // 如果需要滚动条
            scrollbar: {
              el: '.swiper-scrollbar',
            },
          })  
        ```

    - 纵向内容查看

      - ```html
        <div class="swiper-container">
            <div class="swiper-wrapper">
                <div class="swiper-slide">
                    <p>
                        lorem55
                    </p>
                    <p>
                        lorem55
                    </p>
                </div>
            </div>
            <!-- add a scroll bar -->
            <div class="swiper-scrollbar"></div>
        </div>
        ```

      - css配置

      - ```css
        .swiper-slide{
            height:auto;
        }
        ```

      - js配置

      - ```js
            var swiper = new Swiper('.swiper-container', {
              direction: 'vertical',
              slidesPerView: 'auto',
              freeMode: true,
              scrollbar: {
                el: '.swiper-scrollbar',
              },
              mousewheel: true,
            });
        ```



# 10 使用postman测试接口

- 使用postman测试提供的后端文件的地理位置接口

- 在文件里找到的测试地理位置接口, 仅限测试国内地理位置

  - ```js
    /*
    根据经纬度获取位置详情
     */
    router.get('/position/:geohash', function(req, res) {
      const {geohash} = req.params
      ajax(`http://cangdu.org:8001/v2/pois/${geohash}`)
        .then(data => {
          res.send({code: 0, data})
        })
    })
    ```



# 11 封装axios

```js
/* 
    对axios进行二次封装
*/

/* 
    1. 统一处理请求异常
    2. 异步请求成功的数据不是response,而是response.data
    3. 对post请求进行urlencoded处理, 而不是使用默认的json方式
    4. 配置请求超时的时间
    5. 通过请求头携带token数据

*/
import axios from 'axios'
import qs from 'qs'
const instance = axios.create({
	headers: { 'content-type': 'application/x-www-form-urlencoded' },
	timeout: 20000
})

// 请求拦截器
instance.interceptors.request.use(
	config => {
		console.log('req interceptors')
		const data = config.data
		if (data instanceof Object) {
			config.data = qs.stringify(data)
		}

		return config
	},
	error => {}
)

// 响应拦截器
instance.interceptors.response.use(
	response => {
		console.log('res interceptors')
		return response.data
	},
	error => {
		alert('请求出错:' + error.message)
		return Promise.reject(() => {}) // 返回一个pending状态的promse中断promise链
	}
)

export default instance

```

- 在src/ap/ajax.js文件里写入

# 12 测试ajax请求

- 根据服务器里的代码来请求地址

  - ```js
    router.get('/position/:geohash', function(req, res) {
      const {geohash} = req.params
      ajax(`http://cangdu.org:8001/v2/pois/${geohash}`)
        .then(data => {
          res.send({code: 0, data})
        })
    })
    ```

  - 服务器开启地址为 `http://localhost:4000`, 也就是说要向`http://localhost:4000/psoition`发送请求才能获取到地址

- 利用已经封装好的axios来发送请求, 在 src/api/index.js 文件中封装请求地址的方法

  - ```js
    import ajax from './ajax'
    // 1.根据经纬度获取位置详情
    export const reqAddress = (longitude, latitude) => ajax(`/position/${latitude},${longitude}`)
    ```

- 在App.vue文件中发送请求

  - ```js
    import { reqAddress } from '@/api/index'
    ```

  - ```js
    	async mounted () {
    		const result = await reqAddress('116.368647', '40.10038')
    		console.log('result', result)
    	}
    ```

- 打包文件, 发送失败, 产生跨域问题, 原因如下

  - 在ajax.js文件中, instance创建的baseURL为`http://localhost:4000`
  - mounted直接发送请求, 请求为`http://localhost:4000/position/lognitude,latitude`
  - 而客户端的地址是`http://localhost:8080`,由此产生跨域问题

- 配置代理服务器来解决跨域

  - 在ajax.js文件中为baseURL改写

    - ```js
      const instance = axios.create({
      	baseURL: '/api', // ajax请求直接访问 http://localhost:8080/api, 开发服务器设置了代理, 删掉/api路径, 并向http://localhost:4000转发
      	headers: { 'content-type': 'application/x-www-form-urlencoded' }, // 这个不是必须的,处理了数据后会自动修改请求头
      	timeout: 20000
      })
      ```

    - 这样写会直接让客户端从`http://localhost:8080`向`http://localhost:8080/api`发送请求, 不会产生跨域问题

  - 在`vue.config.js`文件中写入

    - ```js
      	devServer: {
      		proxy: {
      			'/api': {
      				target: 'http://localhost:4000', // 转发的目标地址 http://127.0.0.1:4000/api/search/users
      				pathRewrite: {
      					'^/api': '' // 转发请求时去除路径前面的api
      				},
      				changeOrigin: true // 如果主机也不相同,必须加上
      			}
      		},
      		historyApiFallback: true
      	},
      ```

    - 这个代理的意思是: 如果我的请求地址里带有 /api ,则这个地址是向`http://localhost:4000`发送的, 并且转发的时候删除路径里的`/api`

    - 比如刚才的转发流程:

      - 客户端`http://localhost:8080`发起请求`http://localhost:8080/api/position/longititude,latitude`
      - 代理服务器观察到路径中有`/api`,于是移花接木路径为`http://localhost:4000/position/longtitude.latitude`, 由于是服务器向服务器发送请求, 因此没有跨域限制
      - 代理服务器得到请求后再将获得的数据返回, 因此看起就像从`http://localhost:8080`向`http://localhost:4000/api/position`发送请求, 并且成功从`http://localhost:4000/position`访问并获取响应数据

# 13 解决跳转到当前路由的警告

- 即通过`this.$router.replace(url)`跳转路径时. 跳转相同的路径时报错
- 老师给的方法是判断url是否是当前的路径, 不是的话才跳转
- 但有的时候希望点击跳转当前路由的时候实现刷新当前页面
  - 方案一:
    - window.location=path(当前路径) // 发送一般的请求==>整个页面刷新显示

# 14 使用vuex管理状态数据

- 下载vuex

  - `npm i vuex@3.1.2 -S`

- 创建vuex目录

  - ```
    action.js
    getter.js
    mutations.js
    mutations-type.js
    state.js
    index.js
    ```

- index.js主文件里引入各个文件, 并创建

  - ```js
    import Vue from 'vue'
    import Vuex from 'vuex'
    // 引入四个配置文件
    import state from './state'
    import mutations from './mutations'
    import actions from './actions'
    import getters from './getters'
    
    Vue.use(Vuex)
    export default new Vuex.store({
    	state,
    	mutations,
    	actions,
    	getters
    })
    ```

- main.js入口文件里引入管理文件并注册

  - ```js
    import store from '@/vuex/index'
    ```

  - ```js
    new Vue({
        // 省略一些代码
        store:store // 注册管理工具
    })
    ```

  - 在此之后就可以在各个组件里通过`this.$store`来管理数据了

- 定义state

  - store/state.js

  - ```js
    export dafault{
        latitude:40.10038, // 纬度
        longitude:116.368667, // 经度
        address:{}, //地址对象信息
        categorys:[], // 分类数组
        shops:[], // 商家数组
    }
    ```

- 定义mutations-type

  - store/mutations-type.js

  - ```js
    /* 包含mutations函数名常量 */
    export const RECEIVE_ADDRESS = 'receive_address'
    export const RECEIVE_CATEGORYS = 'receive_categorys'
    export const RECEIVE_SHOPS = 'receive_shops'
    ```

  - 事实上我暂时不知道这个文件有什么用

- 定义mutations

  - store/mutations.js

  - ```js
    /* 用于直接更新状态数据方法的对象 */
    import { RECEIVE_ADDRESS, RECEIVE_CATEGORYS, RECEIVE_SHOPS } from './mutations-type'
    const mutations = {
    	/* 直接更新 */
    	[RECEIVE_ADDRESS](state, address) {
    		state.address = address
    	},
    	[RECEIVE_CATEGORYS](state, categorys) {
    		state.categorys = categorys
    	},
    	[RECEIVE_SHOPS](state, shops) {
    		state.shops = shops
    	}
    }
    
    export default mutations
    ```

  - 这里直接引入了mutations-type.js文件里定义的几个大写常量. 

    - 首先确定一点, 不论这些文件在哪里使用, 这些常量都是不变的
    - 也就是说我可以在mutations-type里为这些函数写入各种变量来定义直接操作函数
    - 使用的话是使用this.$store.commit(RECEIVE_ADDRESS)或者this.$store.commit('receive_address')
      - 其实只是用法的不同, mutations中的函数依然被以字符串`receive_address`命名, 但这个字符串同时也被赋值给了RECEIVE_ADDRESS常量, 我的理解是 因为变量有提示而字符串没有提示, 为了避免书写长字符串出错而创建mutations-type.js文件设定常量 

- 定义getters

  - 暂时还没有用的到的地方

- 定义actions

  - ```js
    /* 包含用于间接更新状态数据方法的对象 */
    
    import { reqAddress, reqCategorys, reqShops } from '@/api/index'
    import { RECEIVE_ADDRESS, RECEIVE_CATEGORYS, RECEIVE_SHOPS } from './mutations-type'
    // 这里引入了mutations-type.js文件里定义的常量
    
    const actions = {
    	/* 获取地址信息的异步actions */
    	async getAddress(ctx) {
    		const { longitude, latitude } = ctx.state
    		// 1.发送异步请求
    		const result = await reqAddress(longitude, latitude)
    		// 2.请求成功,提交给mutations
    		if (result.code === 0) {
    			const address = result.data
                // ctx.commit([mutations中定义的])
    			ctx.commit(RECEIVE_ADDRESS, address)
    		}
    	},
    	/* 获取商品信息的异步actions */
    	async getCategorys(ctx) {
    		// 1.发送异步请求
    
    		const result = await reqCategorys()
    		// 2.请求成功,提交给mutations
    		if (result.code === 0) {
    			const categorys = result.data
    			ctx.commit(RECEIVE_CATEGORYS, categorys)
    		}
    	},
    	/* 获取商店地址信息的异步actions */
    	async getShops(ctx) {
    		// 1.发送异步请求
    		const result = await reqShops()
    		// 2.请求成功,提交给mutations
    		if (result.code === 0) {
    			const shops = result.data
    			ctx.commit(RECEIVE_SHOPS, shops)
    		}
    	}
    }
    export default actions
    
    ```

- 在App.vue中发起请求更新数据

  - ```js
    	async mounted () {
    		// this.$store.commit(TEST, 'hello world')
    		// console.log(this.$store)
    		await this.$store.dispatch('getAddress')
    		// console.log(this.$store.state.address)
    	}
    ```

# 15 复习

## ajax封装

- 统一处理请求异常: 响应拦截器的失败的回调
- 异步请求成功的数据不是response, 而是response.data: 响应拦截器的成功回调返回response.data
- 对请求体参数进行urlencoded处理, 而不是使用默认的json方式(后台接口不支持): 请求拦截器中将data对象装换为urlencoded字符串(使用qs库 , Qs.stringify(config.params))
- 解决ajax的跨域问题
  - 配置代理: vue.config.js中配置 devServer.proxy
  - 代理服务器: webpack-dev-server => http-proxy-middleware
  - 作用: 针对前台虚拟路径的特定请求转发请求操作

## vuex编码

- 设计state: 从后台获取的数据
- 实现actions: 
  - 定义异步action: async/await
  - 流程: 发ajax获取数据, commit 给mutation
- 实现mutation: 定义直接更新state数据的函数
- 实现index.js: 创建store对象
- main.js: 配置store
- 组件中:
  - 分发: this.$store.dispatch('actionName',data)
  - 提交: this.$store.commit('mutationName',data)
  - 读取: mapState() / mapGetters()

## 关于面试聊什么

- 技术方面: 原型\闭包\作用域\执行上下文\异步(promise,async,await)\事件循环机制(宏任务/微任务)\ajax(ajax跨域, axios,axios的二次封装,token) 大概十分钟
- vuex



# 16 异步显示当前地址

- 拦截器就是promise

- 前面已经实现打开向服务器发送请求然后获取地址数据, 并由vuex更新到state

- 在Msite.vue中定义vuex的state数据

  - ```js
    // 1. 从vuex引入mapState
    import {mapState} from 'vuex'
    ```

  - ```js
    // 2. 在computed中定义具体的数据
    computed:{
        ...mapState(['address'])
    }
    ```

  - ```html
    // 3. 在模板的<Header>中进行数据绑定
    <Header :title="address.name||'定位中'"></Header>
    ```

- 视频里没有说的部分, 我注意到标题被用省略号省略了, 我猜想可能是lodash插件

- 在.vue文件中使用

  - `import {truncate} from 'lodash'`

  - ```js
    methods:{
        truncate
    }
    ```

  - ```html
    <Header :title="truncate(address.name,{length:10})||'定位中'"></Header>
    ```



## 17 异步显示商家列表

- ```js
  mounted(){
      this.$store.dispatch('getCateorys')
      this.$store.dispatch('getShop')
  }
  ```

- 请求路径: `https://fuss10.elemecdn.com`
