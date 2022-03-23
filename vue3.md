[Toc]

- 引入
  - `<script src="./node_modules/vue/dist/vue.global.js"></script>`

# 1 声明式渲染

- ```html
  <div id="counter">
    Counter: {{ counter }}
  </div>
  ```

- ```js
  const Counter = {
    data() {
      return {
        counter: 0
      }
    },
    mounted() {
      setInterval(() => {
        this.counter++
      }, 1000)
    }
  }
  Vue.createApp(Counter).mount('#counter')
  ```

- 所有实例的data都是返回一个对象?
- 使用`Vue.createApp([ 实例对象变量 ).mount([ html元素id ])`来创建Vue实例,并使用.mount来挂载

# 2 绑定属性

- ```html
  		<div id="bind-attribute">
  			<span v-bind:title="message">鼠标悬停几秒钟查看此处动态绑定的提示信息！</span>
  		</div>
  		<script src="./node_modules/vue/dist/vue.global.js"></script>
  		<script>
  			const AttributeBinding = {
  				data() {
  					return {
  						message: 'You loaded this page on ' + new Date().toLocaleString()
  					}
  				}
  			}
  
  			Vue.createApp(AttributeBinding).mount('#bind-attribute')
  		</script>

- 和vue2没有区别

# 3 方法

- ```html
  		<div id="event-handling">
  			<p>{{ message }}</p>
  			<button v-on:click="reverseMessage">反转 Message</button>
  		</div>
  		<script src="./node_modules/vue/dist/vue.global.js"></script>
  		<script>
  			const EventHandling = {
  				data() {
  					return {
  						message: 'Hello Vue.js!'
  					}
  				},
  				methods: {
  					reverseMessage() {
  						this.message = this.message.split('').reverse().join('')
  					}
  				}
  			}
  
  			Vue.createApp(EventHandling).mount('#event-handling')
  		</script>
  ```

- 和vue2没有区别

# 4 v-model

- 和vue2没有区别

# 5 条件与循环v-if/v-for

- 和vue2没有区别

# 6 组件化应用构建

- ```html
  		<ol id="app">
  			<!-- 创建一个 todo-item 组件实例 -->
  			<todo-item></todo-item>
  		</ol>
  		<script src="./node_modules/vue/dist/vue.global.js"></script>
  		<script>
  			/* 1. 创建一个模板对象, 该对象有一个template属性 */
  			const TodoItem = {
  				template: `<li>This is a todo</li>`
  			}
  			/* 2.创建一个Vue实例 */
  			const app = Vue.createApp({
  				components: {
  					TodoItem // 注册一个新组件
  				}
  			})
  
  			// 挂载 Vue 应用
  			app.mount('#app')
  		</script>
  ```

## 6.1 一个比较完整的组件实例

- ```html
  		<div id="todo-list-app">
  			<ol>
  				<todo-item v-for="item in groceryList" :todo="item" v-bind:key="item.id"></todo-item>
  			</ol>
  		</div>
  		<script src="./node_modules/vue/dist/vue.global.js"></script>
  		<script>
  			/* 1. 创建一个模板对象, 该对象有一个template属性 */
  			const TodoItem = {
  				props: ['todo'], // 子组件内通过props接收来自父组件的数据
  				template: `<li>{{ todo.text }}</li>`
  			}
  			/* 2.创建一个Vue实例 */
  			const app = Vue.createApp({
  				data() {
  					return {
  						groceryList: [
  							{ id: 0, text: 'Vegetables' },
  							{ id: 1, text: 'Cheese' },
  							{ id: 2, text: 'Whatever else humans are supposed to eat' }
  						]
  					}
  				},
  				components: {
  					TodoItem
  				}
  			})
  
  			// 挂载 Vue 应用
  			app.mount('#todo-list-app')
  		</script>
  ```

# 7 应用&组件实例

## 7.1 创建一个应用实例

- 每个 Vue 应用都是通过用 `createApp` 函数创建一个新的**应用实例**开始的：

- 该应用实例是用来在应用中注册“全局”组件的

  - ```js
    const app = Vue.createApp({
      /* 选项 */
    })
    app.component('SearchInput', SearchInputComponent)
    app.directive('focus', FocusDirective)
    app.use(LocalePlugin)
    ```

- 应用实例暴露的大多数方法都会返回该同一实例，允许链式

  - ```js
    Vue.createApp({})
      .component('SearchInput', SearchInputComponent)
      .directive('focus', FocusDirective)
      .use(LocalePlugin)
    ```

## 7.2 根组件

- 如果你想把一个 Vue 应用挂载到 `<div id="app"></div>`，应该传入 `#app`：

  - ```js
    const RootComponent = { 
      /* 选项 */ 
    }
    const app = Vue.createApp(RootComponent)
    const vm = app.mount('#app')
    ```

  - `mount` 不返回应用本身。相反，它返回的是根组件实例。

# 8 组件实例 property

- ```js
  const app = Vue.createApp({
    data() {
      return { count: 4 }
    }
  })
  
  const vm = app.mount('#app')
  
  console.log(vm.count) // => 4
  ```

- 可以通过组件实例`vm.count`来直接访问, 组件实例的所有属性,都可以在组件的模板中访问

- 组件实例还暴露了一些内置属性,以`$`符号开头

# 9 生命周期钩子

![实例的生命周期](https://v3.cn.vuejs.org/images/lifecycle.svg)

# 10 模板语法

## 10.1 插值

- ```html
  <span>Message: {{ msg }}</span>
  ```

- 通过使用 [v-once 指令](https://v3.cn.vuejs.org/api/directives.html#v-once)，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新

  - ```html
    <span v-once>这个将不会改变: {{ msg }}</span>

## 10.2  原始 HTML

- 双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用`v-html`指令

- ```html
  <p>Using mustaches: {{ rawHtml }}</p>
  <p>Using v-html directive: <span v-html="rawHtml"></span></p>
  ```

## 10.3 Attribute

- ```html
  <div v-bind:id="dynamicId"></div>
  ```

- 对于布尔 attribute (它们只要存在就意味着值为 `true`)

  - ```html
    <button v-bind:disabled="isButtonDisabled">按钮</button>

  - 如果该值是一个空字符串，它也会被包括在内，与 `<button disabled="">` 保持一致。对于其他 falsy[[2\]](https://v3.cn.vuejs.org/guide/template-syntax.html#footnote-2) 的值，该 attribute 将被省略。

## 10.4 使用 JavaScript 表达式

- 每个绑定都只能包含**单个表达式**

# 11 指令

## 11.1 参数

- ```html
  <a v-bind:href="url"> ... </a>
  ```

## 11.2 动态参数

- ```html
  <a v-bind:href="url"> ... </a>
  ```

- ```html
  <a v-on:[eventName]="doSomething"> ... </a>
  ```

- ```html
  <a v-on:[eventName]="doSomething"> ... </a>
  ```

## 11.3 修饰符

- ```html
  <form v-on:submit.prevent="onSubmit">...</form>
  ```



# 12 Data属性和methods属性

## 12.1 Data

- ```js
  const app = Vue.createApp({
    data() {
      return { count: 4 }
    }
  })
  
  const vm = app.mount('#app')
  
  console.log(vm.$data.count) // => 4
  console.log(vm.count)       // => 4
  
  // 修改 vm.count 的值也会更新 $data.count
  vm.count = 5
  console.log(vm.$data.count) // => 5
  
  // 反之亦然
  vm.$data.count = 6
  console.log(vm.count) // => 6
  ```

  - 可以通过`vm.$data[属性名]` 获取属性值
  - 可以通过`vm[属性名]`获取属性值

## 12.2 Methods

- ```js
  const app = Vue.createApp({
    data() {
      return { count: 4 }
    },
    methods: {
      increment() {
        // `this` 指向该组件实例
        this.count++
      }
    }
  })
  
  const vm = app.mount('#app')
  
  console.log(vm.count) // => 4
  
  vm.increment()
  
  console.log(vm.count) // => 5
  ```

  - Vue 自动为 `methods` 绑定 `this`，以便于它始终指向组件实例。

### 12.2.1 防抖和节流

- Vue 没有内置支持防抖和节流，但可以使用 [Lodash](https://lodash.com/) 等库来实现。如果某个组件仅使用一次，可以在 `methods` 中直接应用防抖：

- ```html
  <script src="https://unpkg.com/lodash@4.17.20/lodash.min.js"></script>
  <script>
    Vue.createApp({
      methods: {
        // 用 Lodash 的防抖函数
        click: _.debounce(function() {
          // ... 响应点击 ...
        }, 500)
      }
    }).mount('#app')
  </script>
  ```

- 但是，这种方法对于可复用组件有潜在的问题，因为它们都共享相同的防抖函数。为了使组件实例彼此独立，可以在生命周期钩子的 `created` 里添加该防抖函数:

- ```js
  app.component('save-button', {
    created() {
      // 使用 Lodash 实现防抖
      this.debouncedClick = _.debounce(this.click, 500)
    },
    unmounted() {
      // 移除组件时，取消定时器
      this.debouncedClick.cancel()
    },
    methods: {
      click() {
        // ... 响应点击 ...
      }
    },
    template: `
      <button @click="debouncedClick">
        Save
      </button>
    `
  })
  ```

# 13 计算属性和侦听器

- 对于任何包含响应式数据的复杂逻辑，你都应该使用**计算属性**。

## 13.1 计算属性缓存 vs 方法

- 通过在表达式中调用方法可以和计算属性达到同样的效果

- ```html
  <p>{{ calculateBooksMessage() }}</p>
  <script>
  // 在组件中
  methods: {
    calculateBooksMessage() {
      return this.author.books.length > 0 ? 'Yes' : 'No'
    }
  }
  </script>
  ```

- **计算属性将基于它们的响应依赖关系缓存**。计算属性只会在相关响应式依赖发生改变时重新求值。这就意味着只要 `author.books` 还没有发生改变，多次访问 `publishedBookMessage` 时计算属性会立即返回之前的计算结果，而不必再次执行函数。

- 这也同样意味着下面的计算属性将永远不会更新，因为 `Date.now ()` 不是响应式依赖：

  - ```js
    computed: {
      now() {
        return Date.now()
      }
    }



## 13.2 计算属性的setter

- ```js
  computed: {
    fullName: {
      // getter
      get() {
        return this.firstName + ' ' + this.lastName
      },
      // setter
      set(newValue) {
        const names = newValue.split(' ')
        this.firstName = names[0]
        this.lastName = names[names.length - 1]
      }
    }
  }
  ```

# 侦听器

- 当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。
- watch必须监视data中已有的数据属性,而computed则是通过已有的属性计算出新的属性值

- ```html
  <div id="watch-example">
    <p>
      Ask a yes/no question:
      <input v-model="question" />
    </p>
    <p>{{ answer }}</p>
  </div>
  ```

- ```js
    const watchExampleVM = Vue.createApp({
      data() {
        return {
          question: '',
          answer: 'Questions usually contain a question mark. ;-)'
        }
      },
      watch: {
        // 每当 question 发生变化时，该函数将会执行
        question(newQuestion, oldQuestion) {
          if (newQuestion.indexOf('?') > -1) {
            this.getAnswer()
          }
        }
      },
      methods: {
        getAnswer() {
          this.answer = 'Thinking...'
          axios
            .get('https://yesno.wtf/api')
            .then(response => {
              this.answer = response.data.answer
            })
            .catch(error => {
              this.answer = 'Error! Could not reach the API. ' + error
            })
        }
      }
    }).mount('#watch-example')
  ```

## 13.3 计算属性vs侦听器

- ```html
  <div id="demo">{{ fullName }}</div>
  ```

- ```js
  const vm = Vue.createApp({
    data() {
      return {
        firstName: 'Foo',
        lastName: 'Bar',
        fullName: 'Foo Bar'
      }
    },
    watch: {
      firstName(val) {
        this.fullName = val + ' ' + this.lastName
      },
      lastName(val) {
        this.fullName = this.firstName + ' ' + val
      }
    }
  }).mount('#demo')
  ```

- ```js
  const vm = Vue.createApp({
    data() {
      return {
        firstName: 'Foo',
        lastName: 'Bar'
      }
    },
    computed: {
      fullName() {
        return this.firstName + ' ' + this.lastName
      }
    }
  }).mount('#demo')
  ```

# 14 class与style绑定

## 14.1 对象语法

- ```html
  <div :class="{ active: isActive }"></div>
  ```

- `:class` 指令也可以与普通的 `class` attribute 共存

  - ```html
    <div
      class="static"
      :class="{ active: isActive, 'text-danger': hasError }"
    ></div>
    ```

- 计算属性

  - ```js
    data() {
      return {
        isActive: true,
        error: null
      }
    },
    computed: {
      classObject() {
        return {
          active: this.isActive && !this.error,
          'text-danger': this.error && this.error.type === 'fatal'
        }
      }
    }
    ```

## 14.2 数组语法

- ```html
  <div :class="[activeClass, errorClass]"></div>
  ```

- ```js
  data() {
    return {
      activeClass: 'active',
      errorClass: 'text-danger'
    }
  }



## 14.3 在组件中使用

- 当你在带有单个根元素的自定义组件上使用 `class` attribute 时，这些 class 将被添加到该元素中。此元素上的现有 class 将不会被覆盖。

- ```js
  const app = Vue.createApp({})
  
  app.component('my-component', {
    template: `<p class="foo bar">Hi!</p>`
  })
  ```

- ```html
  <div id="app">
    <my-component class="baz boo"></my-component>
  </div>
  ```

- html被渲染为:

  - ```html
    <p class="foo bar baz boo">Hi</p>
    ```

- 如果你的组件有多个根元素，你需要定义哪些部分将接收这个 class。可以使用 `$attrs` 组件 property 执行此操作：

  - ```html
    <div id="app">
      <my-component class="baz"></my-component>
    </div>
    ```

  - ```js
    const app = Vue.createApp({})
    
    app.component('my-component', {
      template: `
        <p :class="$attrs.class">Hi!</p>
        <span>This is a child component</span>
      `
    })
    ```

## 14.4 绑定内联样式

- ```html
  <div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
  ```

- ```js
  data() {
    return {
      activeColor: 'red',
      fontSize: 30
    }
  }
  ```

***

- ```html
  <div :style="[baseStyles, overridingStyles]"></div>
  ```

# 15 条件渲染

## 15.1 v-if



## 15.2 v-else



## 15.3 v-else-if



## 15.4 v-show



## 15.5 v-if与v-show

- `v-if` 是“真正”的条件渲染，因为它会确保在切换过程中，条件块内的事件监听器和子组件适当地被销毁和重建。

- `v-if` 也是**惰性的**：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。

- 相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。

- 一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好

## 15.6 不推荐同时使用v-if与v-show



# 16 列表渲染

## 16.1  用v-for 把一个数组映射为一组元素



## 16.2 在 v-for 里使用对象

- ```html
  		<div id="todo-list-app">
  			<ul id="v-for-object" class="demo">
  				<li v-for="(value, name, index) in myObject">{{ index }}. {{ name }}: {{ value }}</li>
  			</ul>
  		</div>
  ```

- ```js
  Vue.createApp({
    data() {
      return {
        myObject: {
          title: 'How to do lists in Vue',
          author: 'Jane Doe',
          publishedAt: '2016-04-10'
        }
      }
    }
  }).mount('#v-for-object')
  ```

- ![image-20220321172651144](C:\Users\董磊\AppData\Roaming\Typora\typora-user-images\image-20220321172651144.png)

- 在遍历对象时，会按 `Object.keys()` 的结果遍历，但是不能保证它在不同 JavaScript 引擎下的结果都一致。

## 16.3 维护状态

- 当 Vue 正在更新使用 `v-for` 渲染的元素列表时，它默认使用“就地更新”的策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序，而是就地更新每个元素，并且确保它们在每个索引位置正确渲染。

  这个默认的模式是高效的，但是**只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出**。

  为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一的 `key` attribute：

- ```html
  <div v-for="item in items" :key="item.id">
    <!-- 内容 -->
  </div>
  ```

- 尽可能在使用 `v-for` 时提供 `key` attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。



## 16.4 数组更新检测

- Vue 将被侦听的数组的变更方法进行了包裹，所以它们也将会触发视图更新。这些被包裹过的方法包括：

  - `push()`
  - `pop()`
  - `shift()`
  - `unshift()`
  - `splice()`
  - `sort()`
  - `reverse()`

  你可以打开控制台，然后对前面例子的 `items` 数组尝试调用变更方法。比如 `example1.items.push({ message: 'Baz' })`。

## 16.5 替换数组

- 变更方法，顾名思义，会变更调用了这些方法的原始数组。相比之下，也有非变更方法，例如 `filter()`、`concat()` 和 `slice()`。它们不会变更原始数组，而**总是返回一个新数组**。当使用非变更方法时，可以用新数组替换旧数组：

- ```js
  example1.items = example1.items.filter(item => item.message.match(/Foo/))
  ```



## 16.6 显示过滤/排序后的结果

- 有时，我们想要显示一个数组经过过滤或排序后的版本，而不实际变更或重置原始数据。在这种情况下，可以创建一个计算属性，来返回过滤或排序后的数组。

- ```html
  <li v-for="n in evenNumbers" :key="n">{{ n }}</li>
  ```

- ```js
  data() {
    return {
      numbers: [ 1, 2, 3, 4, 5 ]
    }
  },
  computed: {
    evenNumbers() {
      return this.numbers.filter(number => number % 2 === 0)
    }
  }
  ```

- 在计算属性不适用的情况下 (例如，在嵌套的 `v-for` 循环中) 你可以使用一个方法：

- ```html
  <ul v-for="numbers in sets">
    <li v-for="n in even(numbers)" :key="n">{{ n }}</li>
  </ul>
  ```

- ```js
  data() {
    return {
      sets: [[ 1, 2, 3, 4, 5 ], [6, 7, 8, 9, 10]]
    }
  },
  methods: {
    even(numbers) {
      return numbers.filter(number => number % 2 === 0)
    }
  }
  ```



## 16.7 v-for使用整数

- ```html
  <div id="range" class="demo">
    <span v-for="n in 10" :key="n">{{ n }} </span>
  </div>
  ```



## 16.8 在'template'中使用v-for

- ```html
  <ul>
    <template v-for="item in items" :key="item.msg">
      <li>{{ item.msg }}</li>
      <li class="divider" role="presentation"></li>
    </template>
  </ul>
  ```



## 16.9 在组件上使用v-for

- ```html
  <my-component v-for="item in items" :key="item.id"></my-component>
  ```

- 然而，任何数据都不会被自动传递到组件里，因为组件有自己独立的作用域。为了把迭代数据传递到组件里，我们要使用 props：

- ```js
  <my-component
    v-for="(item, index) in items"
    :item="item"
    :index="index"
    :key="item.id"
  ></my-component>
  ```



# 17 事件处理

- 与vue2.x无太大区别

## 17.1 访问事件

- 有时也需要在内联语句处理器中访问原始的 DOM 事件。可以用特殊变量 `$event` 把它传入方法：

- ```html
  <button @click="warn('Form cannot be submitted yet.', $event)">
    Submit
  </button>
  ```

- ```js
  methods: {
    warn(message, event) {
      // 现在可以访问到原生事件
      if (event) {
        event.preventDefault()
      }
      alert(message)
    }
  }
  ```



## 17.2 多个处理事件由逗号隔开

- ```html
  <button @click="one($event), two($event)">
    Submit
  </button>
  ```



## 17.3 事件修饰符

- `.stop`,`.prevent`,`.capture`,`.self`,`.once`,`passive`

- ```html
  <!-- 阻止单击事件继续冒泡 -->
  <a @click.stop="doThis"></a>
  
  <!-- 提交事件不再重载页面 -->
  <form @submit.prevent="onSubmit"></form>
  
  <!-- 修饰符可以串联 -->
  <a @click.stop.prevent="doThat"></a>
  
  <!-- 只有修饰符 -->
  <form @submit.prevent></form>
  
  <!-- 添加事件监听器时使用事件捕获模式 -->
  <!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
  <div @click.capture="doThis">...</div>
  
  <!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
  <!-- 即事件不是从内部元素触发的 -->
  <div @click.self="doThat">...</div>
  
  <!-- 点击事件将只会触发一次 -->
  <a @click.once="doThis"></a>
  
  <!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发，   -->
  <!-- 而不会等待 `onScroll` 完成，                    -->
  <!-- 以防止其中包含 `event.preventDefault()` 的情况  -->
  <div @scroll.passive="onScroll">...</div>
  ```

- 用 `@click.prevent.self` 会阻止**元素本身及其子元素的点击的默认行为**，而 `@click.self.prevent` 只会阻止对元素自身的点击的默认行为。



## 17.4 按键修饰符

- `.enter`,`.tab`,`.delete`,`.esc`,`.space`,`.up`,`.down`,`.left`,`.right`,`.ctrl`,`.alt`,`.shift`,`.meta`

- .exact修饰符

  - ```html
    <!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
    <button @click.ctrl="onClick">A</button>
    
    <!-- 有且只有 Ctrl 被按下的时候才触发 -->
    <button @click.ctrl.exact="onCtrlClick">A</button>
    
    <!-- 没有任何系统修饰符被按下的时候才触发 -->
    <button @click.exact="onClick">A</button>
    ```

## 17.5 鼠标按钮修饰符

- `.left`,`.right`,`.middle`

# 18 表单输入绑定

`v-model` 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：

- text 和 textarea 元素使用 `value` property 和 `input` 事件；
- checkbox 和 radio 使用 `checked` property 和 `change` 事件；
- select 字段将 `value` 作为 prop 并将 `change` 作为事件。



## 18.1 修饰符

- `.lazy`, `.number`, `.trim`



# 19 组件基础

## 19.1 示例

- ```js
  // 创建一个Vue 应用
  const app = Vue.createApp({})
  
  // 定义一个名为 button-counter 的新全局组件
  app.component('button-counter', { // 注册组件
    data() {
      return {
        count: 0
      }
    },
    // 编写组件模板
    template: `
      <button @click="count++">
        You clicked me {{ count }} times.
      </button>`
  })
  ```



## 19.2 通过 Prop 向子组件传递数据

- ```html
  <div id="blog-post-demo" class="demo">
    <blog-post title="My journey with Vue"></blog-post>
    <blog-post title="Blogging with Vue"></blog-post>
    <blog-post title="Why Vue is so fun"></blog-post>
  </div>
  ```

- 

- ```js
  const app = Vue.createApp({})
  
  app.component('blog-post', {
    props: ['title'],
    template: `<h4>{{ title }}</h4>`
  })
  
  app.mount('#blog-post-demo')
  ```

- 上面的是静态数据, :title="msg" 可以传递动态数据

## 19.3 监听子组件事件

- 父组件模板里监听子组件的自定义方法, 调用的是父组件的方法, 子组件的元素通过$emit触发自定义方法



## 19.4 在组件上使用v-model

- ```html
  <input v-model="searchText" />
  ```

- 等价于

  - ```html
    <input :value="searchText" @input="searchText = $event.target.value" />
    ```

- 在组件上使用等价于

  - ```html
    <custom-input
      :model-value="searchText"
      @update:model-value="searchText = $event"
    ></custom-input>
    ```

为了让它正常工作，这个组件内的 `<input>` 必须：

- 将其 `value` attribute 绑定到一个名叫 `modelValue` 的 prop 上
- 在其 `input` 事件被触发时，将新的值通过自定义的 `update:modelValue` 事件抛出

```js
app.component('custom-input', {
  props: ['modelValue'],
  emits: ['update:modelValue'],
  template: `
    <input
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    >
  `
})
```







#  可复用&组合

## 组合式API

## setup组件选项

- `setup` 选项是一个接收 `props` 和 `context` 的函数
- 新的 `setup` 选项在组件创建**之前**执行，一旦 `props` 被解析，就将作为组合式 API 的入口。











































