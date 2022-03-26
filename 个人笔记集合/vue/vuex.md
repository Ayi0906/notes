[Toc]

# 官网

> [Vuex 是什么？ | Vuex (vuejs.org)](https://v3.vuex.vuejs.org/zh/)

- 使用vue2.x搭配的是vuex3.x
- 使用vue3.x搭配的是vuex4.x

# 下载

- `npm i vuex@3.6.2`

# 创建数据状态库

- 在`src`下创建`store`目录
- store中创建`state.js`,`mutations.js`,`actions.js`,`getters.js`,`index.js`五个文件



## state.js:

- ```js
  const state = {
  	count: 0
  }
  
  export default state
  ```

- 

## mutations.js:

- ```js
  const mutations = {
  	increment(state) {
  		state.count++
  	},
  	decrement(state) {
  		state.count--
  	}
  }
  export default mutations
  ```

- `mutations.js`文件中的函数接收一个`state`参数, 可以直接对`state`的数据进行修改, 通过`state.xxx`的方式

- mutations还可以接收额外的参数,它有一个固定名词"载荷(payload)"

- mutations中的函数的调用方式是`this.$store.commit('increment',10)`  // 10 是载荷

- mutations必须是同步函数

## actions.js:

- 接收一个与store实例对象具有相同方法和属性的对象

- ```js
  const actions = {
  	asyncIncrement(context) {
  		setTimeout(() => {
  			context.commit('increment')
  		}, 1000)
  	}
  }
  
  export default actions
  // 或者
  /* 
      asyncIncrment({dispatch,commit}){
          setTimeout(()=>{
              commit('increment')
          })
      }
  */
  ```

- action通过`this.$store.dispatch('increment')`触发

- ```js
  // 以载荷形式分发
  store.dispatch('incrementAsync', {
    amount: 10
  })
  
  // 以对象形式分发
  store.dispatch({
    type: 'incrementAsync',
    amount: 10
  })
  ```

- 组建中分发actions

  - ```js
    import { mapActions } from 'vuex'
    
    export default {
      // ...
      methods: {
        ...mapActions([
          'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`
    
          // `mapActions` 也支持载荷：
          'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
        ]),
        ...mapActions({
          add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
        })
      }
    }
    ```

  - 

## getters.js:

- ```js
  const getters = {
  	oddOrEven(state) {
  		if (state.count % 2 === 0) {
  			return '偶数'
  		} else {
  			return '奇数'
  		}
  	}
  }
  export default getters
  ```

- getters中的函数接收state对象作为第一个参数,也可以再接收一个getters作为第二个参数

- store.js:

  - ```js
    import Vue from 'vue'
    import Vuex from 'vuex'
    // 引入四个配置文件
    import state from './state'
    import mutations from './mutations'
    import actions from './actions'
    import getters from './getters'
    
    Vue.use(Vuex)
    
    export default new Vuex.Store({
    	state,
    	mutations,
    	actions,
    	getters
    })
    ```

## module(模块切割)

- ```js
  const moduleA = {
    state: () => ({ ... }),
    mutations: { ... },
    actions: { ... },
    getters: { ... }
  }
  
  const moduleB = {
    state: () => ({ ... }),
    mutations: { ... },
    actions: { ... }
  }
  
  const store = createStore({
    modules: {
      a: moduleA,
      b: moduleB
    }
  })
  
  store.state.a // -> moduleA 的状态
  store.state.b // -> moduleB 的状态
  ```



## index.js

- ```js
  import Vue from 'vue'
  import Vuex from 'vuex'
  // 引入四个配置文件
  import state from './state'
  import mutations from './mutations'
  import actions from './actions'
  import getters from './getters'
  
  Vue.use(Vuex)
  
  export default new Vuex.Store({
  	state,
  	mutations,
  	actions,
  	getters
  })
  
  ```



# App.vue根组件中使用vuex

- ```vue
  <template>
      <div id="app">
          <p>{{count}}</p>
          <p>{{oddOrEven}}</p>
          <button @click="increment">+</button>
          <button @click="decrement">-</button>
          <button @click="asyncIncrement">1s后+1</button>
      </div>
  </template>
  
  <script>
  import { mapState, mapMutations, mapGetters, mapActions } from 'vuex'
  export default {
      name: 'App',
      computed: {
          ...mapState(['count']),
          ...mapGetters(['oddOrEven'])
      },
      methods: {
          ...mapMutations(['increment', 'decrement']),
          ...mapActions(['asyncIncrement'])
      }
  }
  </script>
  ```



# main.js入口文件中注册使用vuex

- ```js
  import Vue from 'vue'
  import App from './App.vue'
  import store from '@/store/index'
  
  Vue.config.productionTip = false
  
  new Vue({
  	store,
  	render: h => h(App)
  }).$mount('#app')
  ```

- 