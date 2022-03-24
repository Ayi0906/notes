[Toc]

# 官网

> [Vuex 是什么？ | Vuex (vuejs.org)](https://v3.vuex.vuejs.org/zh/)

- 使用vue2.x搭配的是vuex3.x
- 使用vue3.x搭配的是vuex4.x

# 下载

- `npm i vuex@3.6.2`

# 创建数据状态库

- 在`src`下创建`vuex`目录

- vuex中创建`state.js`,`mutations.js`,`actions.js`,`getters.js`,`store.js`五个文件

  - state.js:

    - ```js
      const state = {
      	count: 0
      }
      
      export default state
      ```

    - 

  - mutations.js:

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

  - actions.js:

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
          asyncIncrment({commit}){
              setTimeout(()=>{
                  commit('increment')
              })
          }
      */
      ```

    - action通过`this.$store.dispatch('increment')`触发

  - getters.js:

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

    - 