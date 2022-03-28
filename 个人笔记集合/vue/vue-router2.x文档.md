[Toc]

# 起步

- 将组件component映射到路由, 然后告诉Vue Router在哪里渲染它

## html

- ```html
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
  <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
  
  <div id="app">
    <h1>Hello App!</h1>
    <p>
      <!-- 使用 router-link 组件来导航. -->
      <!-- 通过传入 `to` 属性指定链接. -->
      <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
      <router-link to="/foo">Go to Foo</router-link>
      <router-link to="/bar">Go to Bar</router-link>
    </p>
    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 -->
    <router-view></router-view>
  </div>
  ```

### 总结

- 在html中使用`<router-link to="/home">`来实现作为链接跳转的`<a>`标签
- 在html中使用`<router-view></router-view>`来显示组件

## js

- ```js
  // 0. 如果使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter)
  
  // 1. 定义 (路由) 组件。
  // 可以从其他文件 import 进来
  const Foo = { template: '<div>foo</div>' }
  const Bar = { template: '<div>bar</div>' }
  
  // 2. 定义路由
  // 每个路由应该映射一个组件。 其中"component" 可以是
  // 通过 Vue.extend() 创建的组件构造器，
  // 或者，只是一个组件配置对象。
  // 我们晚点再讨论嵌套路由。
  const routes = [
    { path: '/foo', component: Foo },
    { path: '/bar', component: Bar }
  ]
  
  // 3. 创建 router 实例，然后传 `routes` 配置
  // 你还可以传别的配置参数, 不过先这么简单着吧。
  const router = new VueRouter({
    routes // (缩写) 相当于 routes: routes
  })
  
  // 4. 创建和挂载根实例。
  // 记得要通过 router 配置参数注入路由，
  // 从而让整个应用都有路由功能
  const app = new Vue({
    router
  }).$mount('#app')
  
  // 现在，应用已经启动了！
  ```

### 总结

- 定义component的.vue单文件

  - ```
    /src/pages/Home.vue
    /src/pages/About.vue
    ```

- 在src/routes中定义路由

  - 先引入所有路由组件, vue文件, vueRouter文件

    - ```js
      import Home from '@/pages/Home.vue'
      import Vue from 'vue'
      import VueRouter from 'vue-router'
      ```

  - 再配置路由对象routes

    - ```js
      const routes=[
          {path:'/home',component:Home},
          {path:'/about',component:About},
          {path:'/',redirect:'/home'}
      ]
      ```

  - 如果使用模块化机制编程，导入Vue和VueRouter，要调用

    - ```js
      Vue.use(VueRouter)
      ```

  - 再暴露路由router对象

    - ```js
      export const new VueRouter({
          routes
      })
      ```

- 在main.js入口文件中定义路由

  - ```js
    new Vue({
    	router,
    	render: h => h(App)
    }).$mount('#app')
    ```

# 动态路由匹配  

## (/home/:id通过$route.params.id来匹配)

- 我们有一个 `User` 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染。

- ```js
  const User = {
    template: '<div>User</div>'
  }
  
  const router = new VueRouter({
    routes: [
      // 动态路径参数 以冒号开头
      { path: '/user/:id', component: User }
    ]
  })
  ```

- 现在呢，像 `/user/foo` 和 `/user/bar` 都将映射到相同的路由。

- 使用冒号的动态路由参数会被设置到`this.$route.params`,可以在每一个组件内使用

- ```js
  const User={
      template:'<div>User {{$route.params.id}}</div>'
  }
  ```

- 多段路径参数

  - 

  - | 模式                          | 匹配路径            | $route.params                          |
    | ----------------------------- | ------------------- | -------------------------------------- |
    | /user/:username               | /user/evan          | `{ username: 'evan' }`                 |
    | /user/:username/post/:post_id | /user/evan/post/123 | `{ username: 'evan', post_id: '123' }` |

## 响应路由参数的变化  

### (通过watch:{$route(){}}或者beforeRouteUpdate(){}来监听路由,避免动态路由造成的组件使用重复)

- 当使用路由参数时，例如从 `/user/foo` 导航到 `/user/bar`，**原来的组件实例会被复用**。

  - 实测动态路由的组件确实会被复用, input输入框中的文本不会被清除

- 对路由参数的变化作出响应

  - 监听$route的变化:

  - ```js
    const User = {
      template: '...',
      watch: {
        $route(to, from) {
          // 对路由变化作出响应...
        }
      }
    }
    ```

  - `beforeRouteUpdate`导航守卫:

  - ```js
    const User = {
      template: '...',
      beforeRouteUpdate(to, from, next) {
        // react to route changes...
        // don't forget to call next()
        next()
      }
    }
    ```

## 捕获所有路由或404notfound路由  

### (path:'*'匹配所有路由,$route.params.pathMatch匹配路径)

- 常规参数只会匹配被 `/` 分隔的 URL 片段中的字符。如果想匹配**任意路径**，我们可以使用通配符 (`*`)：

- ```js
  {
    // 会匹配所有路径
    path: '*'
  }
  {
    // 会匹配以 `/user-` 开头的任意路径
    path: '/user-*'
  }
  ```

- 含有通配符的路由必须写在最后

- 通配符匹配到路由时,`$route.params`中会自动添加一个名为`pathMatch`参数

  - ```js
    // 给出一个路由 { path: '/user-*' }
    this.$router.push('/user-admin')
    this.$route.params.pathMatch // 'admin'
    // 给出一个路由 { path: '*' }
    this.$router.push('/non-existing')
    this.$route.params.pathMatch // '/non-existing'
    ```

## 匹配优先级

- 有时候，同一个路径可以匹配多个路由，此时，匹配的优先级就按照路由的定义顺序：路由定义得越早，优先级就越高。



# 嵌套路由

- children属性

- ```js
  const router = new VueRouter({
    routes: [
      {
        path: '/user/:id',
        component: User,
        children: [
          {
            // 当 /user/:id/profile 匹配成功，
            // UserProfile 会被渲染在 User 的 <router-view> 中
            path: 'profile',
            component: UserProfile
          },
          {
            // 当 /user/:id/posts 匹配成功
            // UserPosts 会被渲染在 User 的 <router-view> 中
            path: 'posts',
            component: UserPosts
          }
        ]
      }
    ]
  })
  ```



# 编程式的导航

## 点击`<router-link>`相当于调用`this.$router.push(...)`

- 另一种定义链接的方式

- ```js
  this.$router.push(location,onComplete?.onAbort?)
  ```

- 想要导航到不同的 URL，则使用 `router.push` 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

- 当你点击 `<router-link>` 时，这个方法会在内部调用，所以说，点击 `<router-link :to="...">` 等同于调用 `router.push(...)`

- ```js
  // 字符串
  router.push('home')
  
  // 对象
  router.push({ path: 'home' })
  
  // 命名的路由
  router.push({ name: 'user', params: { userId: '123' }}) // 路由可以命名, 后面补充
  
  // 带查询参数，变成 /register?plan=private
  router.push({ path: 'register', query: { plan: 'private' }})
  ```

- 如果提供了path, 则params会被忽略

- 不可以跳到两个相同路由中去

## router.replace(location, onComplete?, onAbort?)

- router.replace不会向history添加新记录, 而是替换掉当前的history记录, 也就是说无法按通过router.go(-1)回到之前的页面

## router.go(n)

```js
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)

// 后退一步记录，等同于 history.back()
router.go(-1)

// 前进 3 步记录
router.go(3)

// 如果 history 记录不够用，那就默默地失败呗
router.go(-100)
router.go(100)
```



## 操作history

-  `router.push`、 `router.replace` 和 `router.go` 跟 [`window.history.pushState`、 `window.history.replaceState` 和 `window.history.go`](https://developer.mozilla.org/en-US/docs/Web/API/History)相似



# 命名路由

- 命名:

  - ```js
    {
        name:'news',
        path:'/home/news',
        component:News
    }
    ```

- 使用:

  - ```html
    <router-link :to="{name:'news'}"></router-link>
    ```

### /home路由自动打开/home/news

- ```js
  {
      path:'/home',
      component:Home,
      children:[
          {path:'',redirect:'/home/news'}
      ]
  }
  ```

### 动态命名路由

- ```js
  {
      name:'detail',
      path:'/home/message/detail/:id',
      component:detail
  }
  ```

- ```html
  <router-link :to="{name:'detail',params:{id:item.id}}"></router-link>
  ```

- ```js
  this.$router.push({name:'detail',params:{id:itme.id}})
  ```





# 命名视图

- 如果 `router-view` 没有设置名字，那么默认为 `default`

- ```html
  <router-view class="view one"></router-view>
  <router-view class="view two" name="a"></router-view>
  <router-view class="view three" name="b"></router-view>
  ```

- 一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 `components` 配置 (带上 s)

- ```js
  const router = new VueRouter({
    routes: [
      {
        path: '/',
        components: {
          default: Foo,
          a: Bar,
          b: Baz
        }
      }
    ]
  })
  ```

### 嵌套命名视图

- 在一个组件中使用多个其它组件需要定义好路由和子组件

- ```js
  {
    path: '/settings',
    // 你也可以在顶级路由就配置命名视图
    component: UserSettings,
    children: [{
      path: 'emails',
      component: UserEmailsSubscriptions
    }, {
      path: 'profile', // 在/profile路由中显示userProfile组件以及userProfileView组件
      components: {
        default: UserProfile,
        helper: UserProfilePreview
      }
    }]
  }
  ```

- ```html
  <div>
    <h1>User Settings</h1>
    <NavBar/>
    <router-view/>
    <router-view name="helper"/>
  </div>
  ```



# 重定向和别名

- ```js
  const router = new VueRouter({
    routes: [
      { path: '/a', redirect: '/b' }
    ]
  })
  ```

- 动态返回重定向目标

  - ```js
    const router = new VueRouter({
      routes: [
        { path: '/a', redirect: to => {
          // 方法接收 目标路由 作为参数
          // return 重定向的 字符串路径/路径对象
        }}
      ]
    })
    ```

- “重定向”的意思是，当用户访问 `/a`时，URL 将会被替换成 `/b`，然后匹配路由为 `/b`，那么“别名”又是什么呢？

  **`/a` 的别名是 `/b`，意味着，当用户访问 `/b` 时，URL 会保持为 `/b`，但是路由匹配则为 `/a`，就像用户访问 `/a` 一样。**

- ```js
  const router = new VueRouter({
    routes: [
      { path: '/a', component: A, alias: '/b' }
    ]
  })
  ```



# 路由组件传参

- 在动态路由声明中

  - ```js
    {
        name:'detail',
        path:'/home/message/detail/:id',
        component:Detail,
        props:true
    }
    ```

- 在对应的组件中声明对应的props

  - ```js
    export default {
        props:['id']
    }
    ```

- 然后这个传入的id属性就可以被使用了???

### 对于有命名视图的路由，你必须为每个命名视图定义 `props` 配置

- ```js
  const routes = [
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
  ```



# history模式

```js
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

- 这个配置会让前端URL看起来就像正常的url,比如`http://yoursite.com/user/id`

- 不过这种模式要玩好，还需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问 `http://oursite.com/user/id` 就会返回 404，这就不好看了。

  所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 `index.html` 页面，这个页面就是你 app 依赖的页面。

- 建议设置一个404页面

  - ```js
    const router = new VueRouter({
      mode: 'history',
      routes: [
        { path: '*', component: NotFoundComponent }
      ]
    })
    ```



# 导航守卫

## 全局前置守卫---beforeEach

- 你可以使用 `router.beforeEach` 注册一个全局前置守卫：

- ```js
  const router = new VueRouter({ ... })
  
  router.beforeEach((to, from, next) => {
    // ...
  })
  ```

- 当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 **等待中**。

- 每个守卫方法接收三个参数：

  - **`to: Route`**: 即将要进入的目标 [路由对象](https://v3.router.vuejs.org/zh/api/#路由对象)

  - **`from: Route`**: 当前导航正要离开的路由

  - **`next: Function`**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。

    - **`next()`**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed** (确认的)。
    - **`next(false)`**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
    - **`next('/')` 或者 `next({ path: '/' })`**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 [`router-link` 的 `to` prop](https://v3.router.vuejs.org/zh/api/#to) 或 [`router.push`](https://v3.router.vuejs.org/zh/api/#router-push) 中的选项。
    - **`next(error)`**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 [`router.onError()`](https://v3.router.vuejs.org/zh/api/#router-onerror) 注册过的回调。

  - **确保 `next` 函数在任何给定的导航守卫中都被严格调用一次。它可以出现多于一次，但是只能在所有的逻辑路径都不重叠的情况下，否则钩子永远都不会被解析或报错**。这里有一个在用户未能验证身份时重定向到 `/login` 的示例：

    - ```js
      router.beforeEach((to, from, next) => {
        if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
        else next()
      })
      ```

## 全局解析守卫---router.beforeResolve

- 在 2.5.0+ 你可以用 `router.beforeResolve` 注册一个全局守卫。这和 `router.beforeEach` 类似，区别是在导航被确认之前，**同时在所有组件内守卫和异步路由组件被解析之后**，解析守卫就被调用。

## 全局后置钩子---afterEach

- 你也可以注册全局后置钩子，然而和守卫不同的是，这些钩子不会接受 `next` 函数也不会改变导航本身：

- ```js
  router.afterEach((to, from) => {
    // ...
  })
  ```

## 路由独享的守卫

- ```js
  const router = new VueRouter({
    routes: [
      {
        path: '/foo',
        component: Foo,
        beforeEnter: (to, from, next) => {
          // ...
        }
      }
    ]
  })
  ```

- 与全局前置守卫的方法参数是一样的

## 组件内的守卫

- ```js
  const Foo = {
    template: `...`,
    beforeRouteEnter(to, from, next) {
      // 在渲染该组件的对应路由被 confirm 前调用
      // 不！能！获取组件实例 `this`
      // 因为当守卫执行前，组件实例还没被创建
    },
    beforeRouteUpdate(to, from, next) {
      // 在当前路由改变，但是该组件被复用时调用
      // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
      // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
      // 可以访问组件实例 `this`
    },
    beforeRouteLeave(to, from, next) {
      // 导航离开该组件的对应路由时调用
      // 可以访问组件实例 `this`
    }
  }
  ```



## 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。



# 路由元信息

- 定义路由的时候可以配置 `meta` 字段：

- ```js
  const router = new VueRouter({
    routes: [
      {
        path: '/foo',
        component: Foo,
        children: [
          {
            path: 'bar',
            component: Bar,
            // a meta field
            meta: { requiresAuth: true }
          }
        ]
      }
    ]
  })
  ```

- 通过$route.meta.requiresAuth来访问自定义元信息

