[Toc]

# 二十一 组件与复用

## 21.1 全局注册组件的方法

1. 全局注册

```javascript
Vue.component('my-component',{ 
	template:'<div>这是组件的内容</div>' 
})
```

2. 在Vue实例对象中使用

```html
<div id="app">
    <my-component></my-component>
</div>
```

3. 备注
   1. vue2.x中必须有一个根节点,可以是`div`也可以是其它元素, 如果只有一个元素可以直接写

## 21.2 局部注册组件的方法

1. 注册及使用

```html
<div id = "app">
    <my-component></my-component>
</div>
```

```javascript
<script>
    var child={
        template:'<div>局部注册组件的内容</div>'
    }
	var app=new vue({
        components:{
            'my-component':child // 接收一个带template属性的对象
        }
    }).$mount('#app')
</script>
```



# 二十二 使用props传递数据



# 二十三 单项数据流



# 二十四 数据验证



# 二十五 组件通信



# 四十六 组件间通信_slot插槽

1. 父组件向子组件通信方式
2. 与props的区别: props传递数据, slot传递标签

## 46.1 使用方法

1. 在组件模板中通过`<slot name="[插槽名称]">`定义插槽,以Header.vue为例

```html
<template>
    <div>
    	<slot name="top"></slot>
		<slot name="middle"></slot>
        <slot name="bottom"></slot>
	</div>
</template>
```



2. 在父组件中使用该子组件,通过`<标签 slot="[插槽名称]"></标签>`来替换

```html
<template>
	<Header>
    	<span slot="top">hello world</span>
        <div slot="middle">
            <input type="text">
            <input type="password">
        </div>
        <div slot="bottom">
            <p>
             	@copyright   
            </p>
            <p>
                lorem25
            </p>
        </div>
    </Header>
</template>
```

3. 总结

   1. 对于组件来说, 只要直接使用`<Header></Header>`就可以在那里引入全部的Header组件的模板

   2. 所以如果在一个模板标签内写入的其它标签,必然是slot插槽的值

   3. 3.13补充:

      1. `<slot>`标签本身不可以写类名, 因为它只是一个位置标识

      ```html
              <slot name="left"></slot>
              <span class="header_title">
                  <!-- 显示不下予以省略 -->
                  <span class="header_title_text ellipsis">{{ title }}</span>
              </span>
              <slot name="right"></slot>
      ```

      2. App.vue父组件中引入子组件, 再设置类名

      ```vue
              <Header :title="'正在定位中'">
                  <span slot="left" class="header_search">
                      <i class="iconfont icon-search"></i>
                  </span>
                  <span slot="right" class="header_login">
                      <span class="header_login_text">登录 | 注册</span>
                  </span>
              </Header>
      ```

      3. 样式文件不可以写在App.vue中,只能写在Header.vue中

# 路由

## 1 vue-router的介绍

- 下载
  - `npm i vue-router@3.5.3`(vue@2.x只能搭配3.5.3)

## 2 路由的基本使用

1. 在src/page目录保存路由页面

`src/page/About.vue   src/page/Home.vue`

2. 在src/router目录中配置路由

`src/router/index.js`

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import About from '@/pages/About'
import Home from '@/pages/Home'
Vue.use(VueRouter)  // 声明使用vueRouter插件
export default new VueRouter({
    routes:[
        {path:'/about',component:About},
        {path:'/home',component:Home},
        {path:'/',redirect:'/about'} // 重定向
    ]
})
```

3. 在入口文件index.js中注册路由

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
new Vue({
    el:'#root',
    components:{App},
    template:'<App/>',
    router:router
})
```

4. 在App.vue文件中使用路由

```vue
<router-link to="/about">about</router-link> <!-- <router-link/>经过vue处理后会转换为<a/>标签 -->
<router-view></router-view> <!-- 使用router-view作为路由显示页面 -->
```

## 3 子路由

1. 在src/pages目录中添加Home_News.vue与Home_Message.vue作为子路由

2. 在src/router目录中修改

   1. 添加子路由引入

      1. ```js
         import Home_Message from '@/pages/Home_Message'
         import Home_News from '@/pages/Home_News'
         ```

   2. 添加/home的子路由

      1. ```js
         {
             path:'/home',
             component:Home,
                 children:[
                     {path:'/home/news',
                      component:Home_News},
                     {path:'/home/message',
                      component:Home_Message}
                 ]
         }

## 4 向路由组件传递数据

1. 在src/pages中定义Home_message.vue的子路由Home_Message_detail.vue
2. 在src/router/index.js中引入,注册路由

```js
import Home_Message_detail from '@/pages/Home_Message_detail'
```

3. 注册

```js
{
    path:'/home/message',
    component:'Home_Message',
    children:[
        {path:'/home/message/detail/:id',
        component:Home_Message_detail}
    ]
}
```

4. 修改Home_Message.vue的模板

```html
<li v-for="item in message" :key="item.id">
	<router-link :to="'/home/message/detail'+item.id">{{item.title}}</router-link> // 动态路由
</li>
```

5. Home_Message_detail.vue模板中获取链接传入的动态数据

```vue
<ul>
    <li>{{$route.params.id}}</li>
</ul>
```

# 5 动态路由及监视(响应路由参数的变化)

- 从`/home/message/detail/3`向`/home/message/detail/5`跳转的过程中,Home_Message_detail组件被复用,生命周期钩子函数不被调用,显示内容也不被调用

```js
created(){
	this.$watch(
    	()=>this.$route.params,
        (toParams,previousParams)=>{}
    )
}
```

# 6 编程式路由导航

- [编程式的导航 | Vue Router (vuejs.org)](https://v3.router.vuejs.org/zh/guide/essentials/navigation.html)

## 6.1 编程式路由基本用法

- 声明式---push---`<router-link :to='/home/message/detail/${id}'>`

  - 编程式

  - ```vue
    <button @click="handlePush(item.id)"></button>
    
    <script>
    handlePush(id){
        if(this.id===id){
            return console.log('不能两次push一样的路由')
        }
        this.$router.push(`/home/message/detail/${id}`)
    }
    </script>
    ```

- 声明式---replace---`<router-link :to='/home/message/detail/${id}' replace>`

  - 编程式

  - ```vue
    <button @click="handlePush(item.id)"></button>
    
    <script>
    handlePush(id){
        if(this.id===id){
            return console.log('不能两次replace一样的路由')
        }
        this.$router.replace(`/home/message/detail/${id}`)
    }
    </script>
    ```

- router.push与router.replace的不同

  - ```vue
    <button @click="$router.back()"></button>
    ```

  - `push`进的路由会添加到历史栈中, 可以通过`$router.back()`返回

  - `replace`的路由会被替代, 历史栈无法找到之前的路由

## .vue单文件的stylus预编译移动端适配

### 1.将stylus预编译文件写在.vue文件内的`<style>`标签内

- ```javascript
  <style scoped lang="stylus" rel="stylesheet/stylus">
  ```

### 2.vue脚手架中使用第三方库适配

- 安装:
  - `yarn add postcss-px2rem lib-flexible`
  - 或者`npm i postcss-px2rem lib-flexible`
  
- 在`main.js`中引入lib-flexible
  - `import 'lib-flexible/flexible'`
  - 可以直接写`import 'lib-flexible'`,
    - 原因是因为`lib-flexible`目录中只有一个`flexible.js`作为主文件,
    - 而且在`package.json`中`"main": "flexible.js",`表明了主文件的名称
  
- 在`vue.config.js`中添加

  - ```javascript
    const px2rem = require('postcss-px2rem')
    const postcss = px2rem({
        // 基准大小, baseSize需要和rem.js中单位rem值占比一样
        remUnit: 37.5, // 设计稿等分之后的值, 等分的比例同页面rem的比例是一致的, 如果设计稿750, 写75
    })
    module.exports = {
        css: {
            // 添加postcss配置
            loaderOptions: {
                postcss: {
                    plugins: [postcss],
                },
            },
        },
    }
    ```

- 使用说明

  - 根据设计稿大小来制定基准remUnit
  - 对于`<style>`标签或者其它`.styl`文件内写入的stylus文件,内大小可以直接写设计稿上了测量的大小
    - 比如一个750px的设计稿上有一个100px宽的盒子 ,那么`remUnit`写75,stylus文件中直接写100px

### 3.解决移动端点击响应0.3秒延时问题

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