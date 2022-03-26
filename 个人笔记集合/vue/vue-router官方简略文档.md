[Toc]

# 下载

- `npm i vue-router@3.5.3`
  - vue2.x只能搭配3.5.3



# 基本使用

- 在`src/pages`中写入路由文件About.vue和Home.vue

- 在`src/router/index.js`中配置路由

  - ```js
    import Vue from 'vue'
    import VueRouter from 'vue-router'
    import About from '@/pages/About'
    import Home from '@/pages/Home'
    Vue.use(VueRouter) // 声明使用vue插件
    
    
    export default new VueRouter({
        routes:[
            {path:'/about',component:About},
            {path:'/home',component:Home},
            {path:'/',redirect:'/about'} // 重定向
        ]
    })
    ```

- 在入口文件index.js中注册路由

  - ```js
    import Vue from 'vue'
    import App from './App.vue'
    import router from './src/router/index.js'
    
    new Vue({
        el:'#root',
        components:{App},
        template:'<App/>',
        router
    })
    ```

- 使用<router-link>展示路由

  - ```html
    <router-link to="/home">home</router-link>
    <router-link to="/about">about</router-link>
    ```

  - router-link会被编译为a标签

- 使用<router-view>显示路由文件

  - ```html
    <router-view></router-view>
    ```

  - `<router-view>`会被转化成div标签

# 子路由

- 声明子路由

  - ```js
    import Home_Message from '@/pages/Home_Message'
    import Home_News from '@/pages/Home_News'
    ```

  - ```js
    {
        path:'/home',
     	component:Home,
        children:[{
            path:'/home/news', // 也可以直接path:'news'
            component:Home_News
        },{
            path:'/home/message',
            component:Home_Message
        }]
    }
    ```

  - 