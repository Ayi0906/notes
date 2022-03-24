[Toc]

# 一个简单的vue计数应用

- ```js
  new Vue({
    // state
    data () {
      return {
        count: 0
      }
    },
    // view
    template: `
      <div>{{ count }}</div>
    `,
    // actions
    methods: {
      increment () {
        this.count++
      }
    }
  })
  ```

- state: 驱动应用的数据源    (静态数据)

- view: 以声明方式将state映射到视图    (在视图上展示静态数据)

- actions: 响应在view上的使用户输入导致的状态变化  (通过视图上的事件触发来修改静态数据源)

  - state的声明会触发view对视图的更新
  - view对事件的监听会触发对应的actions
  - actions会修改state
  - state的修改会触发view对视图的更新

- 在`多个组件共享状态`时使用vuex



# 安装

- `npm i vuex@3.6.2`   (目前我还在使用vue2.x)

- 入口文件中显式地通过 `Vue.use()` 来安装 Vuex：

  - ```js
    import Vue from 'vue'
    import Vuex from 'vuex'
    
    Vue.use(Vuex)
    ```

# 最简单的Store

- ```js
  import Vue from 'vue'
  import Vuex from 'vuex'
  
  Vue.use(Vuex)
  
  const store = new Vuex.Store({
    state: {
      count: 0
    },
    mutations: {
      increment (state) {
        state.count++
      }
    }
  })
  ```

- 通过`store.state`获取状态对象

- 通过`store.commit`触发状态变更

  - ```js
    store.commit('increment')
    
    console.log(store.state.count) // -> 1
    ```

- 可以直接在组件内部声明store

  - ```js
    new Vue({
      el: '#app',
      store,
    })
    ```

- 通过组件的methods提交一个变更

  - ```js
    methods: {
      increment() {
        this.$store.commit('increment')
        console.log(this.$store.state.count)
      }
    }
    ```

  - 我们通过提交 mutation 的方式，而非直接改变 `store.state.count`，是因为我们想要更明确地追踪到状态的变化

# 核心概念

- state

  - Vuex 使用**单一状态树**——是的，用一个对象就包含了全部的应用层级状态

  - 在 Vue 组件中获得 Vuex 状态

    -  Vuex 的状态存储是响应式的，从 store 实例中读取状态最简单的方法就是在计算属性中返回某个状态：

    - ```js
      // 创建一个 Counter 组件
      const Counter = {
        template: `<div>{{ count }}</div>`,
        computed: {
          count () {
            return store.state.count
          }
        }
      }
      ```

    - 每当 `store.state.count` 变化的时候, 都会重新求取计算属性，并且触发更新相关联的 DOM。

    - 但是太麻烦了. 通过直接在根实例中注册 `store` 选项，该 store 实例会注入到根组件下的所有子组件中，且子组件能通过 `this.$store` 访问到。

      - ```js
        // 根组件中定义store
        const app = new Vue({
          el: '#app',
          // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
          store,
          components: { Counter },
          template: `
            <div class="app">
              <counter></counter>
            </div>
          `
        })
        ```

      - ```js
        // 子组件中直接使用 this.$store 访问
        const Counter = {
          template: `<div>{{ count }}</div>`,
          computed: {
            count () {
              return this.$store.state.count
            }
          }
        }
        ```

  - mapState 辅助函数

    - ```js
      computed:{
      	...mapState(['count'])
      }
      ```

    - 然后就可以直接写count了





















