[Toc]

# 1.vsCode配置import @符号 路径提示

1.安装`Path Intellisense`插件, 在`setting.json`中添加, 使得vscode可以通过`@`和`@components`来路径提示

```json
    "path-intellisense.mappings": {
        "@":"${workspaceRoot}/src",
        "@components":"${workspaceRoot}/src/components"
    },
```

2.在项目package.json同级目录中新建文件`jsconfig.json`

- (这一步写与不写似乎区别不大,都可以正确打包)

```json
{ 
    "compilerOptions": { 
        "target": "ES6", 
        "module": "commonjs", 
        "allowSyntheticDefaultImports": true, 
        "baseUrl": "./", 
        "paths": { "@/*": ["src/*"] } 
    }, 
    "exclude": [ "node_modules" ] 
}
```

3.在webpack.config.js中配置路径解析

```javascript
module.exports={
    // some code
    resolve: {
        extensions: ['.js', '.vue', '.json'], // 可以省略的后缀名
        alias: { // 路径别名
            'vue$': 'vue/dist/vue.esm.js', // 表示精准匹配 带vue编辑器
            '@': resolve(__dirname, 'src'), // 路径别名, 解析以@符号开头的路径
            // 需要注意的是, 如果是在config中写入的webpack.dev.js之类的文件, 地址应该是 resolve(__dirname, '../src')
            '@components': resolve(__dirname, 'src/components') // 路径别名, 解析以@components开头的路径
        }
    }
}
```

# 2.自定义PubSub

```javascript
/* 调用 */
// 1.定义回调函数容器
let callbackContainer = {}

// 2.定义回调函数容器的数据结构
/**
 * {
 *     "add":{
 *          "uid_1": callback1,
 *          "uid_2": callback2
 *      },
 *      "delete":{
 *          "uid_3":callback3
 *      }
 * }
 */

// 3.定义PubSub对象
let PubSub = {}

// 4.定义token
let tokenNum = 0

// 5.定义subscribe方法
/**
 * 
 * @param {*} msg 订阅内容
 * @param {*} callback 接收两个参数 : 订阅内容msg 与 发布内容data
 * @returns 
 */
PubSub.subscribe = function (msg, callback) {
    // 1.验证callbackContainer[msg]是否存在,不存在则创建对象
    if (!callbackContainer[msg]) {
        callbackContainer[msg] = {}
    }
    // 2.创建这个订阅的唯一token
    let token = 'uid_' + ++tokenNum

    // 3.添加方法到里面去
    callbackContainer[msg][token] = callback

    // 4.返回token
    return token
}

// 6.定义异步发布publish方法
/**
 * 
 * @param {*} msg 订阅内容
 * @param {*} data 发布内容 
 */
PubSub.publish = function (msg, data) {
    // 1.确定相应的回调函数
    let callbacks = callbackContainer[msg]

    // 2.调用该 订阅内容 中的所有回调
    if (callbacks) {
        Object.values(callbacks).forEach(item => {
            setTimeout(() => {
                item(msg, data)
            });
        })
    }
}


// 7.定义同步发布内容

PubSub.publishSync = function (msg, data) {
    // 1.确定相应的回调函数
    let callbacks = callbackContainer[msg]

    // 2.调用该 订阅内容 中的所有回调
    if (callbacks) {
        Object.values(callbacks).forEach(item => {
            item(msg, data)
        })
    }
}

// 8.定义 根据flag取消订阅
/**
 * 
 * @param {*} flag 可能是token , 也可能是订阅内容
 */
PubSub.unsubscribe = function (flag) {
    // 1.判断flag
    if (typeof flag === 'string' && flag.indexOf('uid_') === 0) {
        // token
        Object.values(callbackContainer).forEach(item => {
            if (item[flag]) {
                delete item[flag]
                return
            }
        })
    } else if (flag === undefined) {
        // 删除全部
        callbackContainer = {}
    } else {
        // msg
        delete callbackContainer[flag]
    }
}

// 订阅
PubSub.subscribe('add', (msg, data) => {
    console.log('add1()', data)
})

const token1 = PubSub.subscribe('add', (msg, data) => {
    console.log('add2()', data)
})

PubSub.subscribe('hello', (msg, data) => {
    console.log('sync', data)
})

PubSub.unsubscribe(token1)

PubSub.publish('add', 'hello add')
PubSub.publishSync('hello', 'hello sync')
```

# 3.自定义事件总线

```javascript
/* 
    确定方法:
    eventBus.on(eventName, callback) : 绑定事件监听
    eventBus.emit(eventName, data) : 触发事件并给予数据
    eventBus.off(eventName || '') : 取消事件监听
    eventBus.on(eventName, callback) : 绑定事件监听, 调用一次后删除该监听

*/

/* 
    确定eventBus的数据结构
    eventBus:{
        "add":{
            "token_1":callback1,
            "token_2":callback2
        },
        "delete":{
            "token_3":calllback3
        },
        "update":{
            "token_once_4":callback4
        }
    }
*/

// 1. 定义eventBus
const eventBus = {}

// 2.定义回调函数容器
let callbackContainer = {}

// 3.定义回调函数的唯一标识
let tokenNum = 0

// 3.定义监听
/**
 * 
 * @param {*} eventName 字符串
 * @param {*} callback 接收两个参数 e 和 data
 */
eventBus.on = function (eventName, callback) {
    if (!callbackContainer[eventName]) {
        callbackContainer[eventName] = {}
    }

    let token = "token_" + ++tokenNum
    callbackContainer[eventName][token] = callback
    return token
}


// 4.定义触发
eventBus.emit = function (eventName, data) {
    let callbacks = callbackContainer[eventName]
    if (callbacks) {
        Object.keys(callbacks).forEach(item => {
            callbacks[item](eventName, data)
            if (item.indexOf('once') !== -1) {
                // 一次性回调
                delete callbacks[item]
            }
        })
    }
}

// 5.定义取消监听
eventBus.off = function (eventNameOrToken) {
    if (eventNameOrToken === undefined) {
        // 清除全部
        callbackContainer = {}
    } else if (typeof eventNameOrToken === 'string' && eventNameOrToken.indexOf('token_') === 0) {
        // token
        Object.values(callbackContainer).forEach(callbacks => {
            if (callbacks[eventNameOrToken]) {
                delete callbacks[eventNameOrToken]
                return
            }
        })
    } else {
        // eventName
        delete callbackContainer[eventNameOrToken]
    }
}

// 6.定义一次性监听
eventBus.once = function (eventName, callback) {
    if (!callbackContainer[eventName]) {
        callbackContainer[eventName] = {}
    }

    let token = "token_once_" + ++tokenNum
    callbackContainer[eventName][token] = callback
    return token
}

// 测试调用
eventBus.on('add', function (e, data) {
    console.log(e, data, '第一个add监听回调')
})

eventBus.on('add', function (e, data) {
    console.log(e, data, '第二个add监听回调')
})

// 触发add事件
eventBus.emit('add', 'hello add')


// 测试取消回调
eventBus.on('update', function (e, data) {
    console.log(e, data, '第一个update监听回调')
})

eventBus.off('update')

eventBus.emit('update', 'haha')

// 测试一次性监听
eventBus.once('get', function (e, data) {
    console.log(e, data, '第一个get回调')
})

eventBus.emit('get', '第一次调用get回调')
eventBus.emit('get', '第二次调用get回调')
```

# 4.使用axios与后台通信

## 完整代码

- 下载vue-resource与axios

  - `npm i vue-resource`
  - `npm i axios`

- index.js

  - ```javascript
    import Vue from 'vue'
    import VueResource from 'vue-resource'
    import App from './App.vue'
    
    // 声明使用Vue插件,内部给所有组件对象都添加了一个属性对象: $http
    Vue.use(VueResource)
    
    new Vue({
        el: '#root',
        data: {
    
        },
        components: {
            App
        },
        template: '<App/>'
    })
    ```

- App.vue

  - ```vue
    <template>
      <div>
        <p v-if="!repoName">loading...</p>
        <p v-else>
          most star repo is
          <a :href="repoUrl">{{ repoName }}</a>
        </p>
      </div>
    </template>
    
    <script>
    // axios需要在.vue内引入 而vue-resource在index.js中引入
    import axios from "axios";
    export default {
      data() {
        return {
          repoName: "",
          repoUrl: "",
        };
      },
      mounted() {
        // this.$http
        //   .get("https://api.github.com/search/repositories?q=v&sort=stars")
        //   .then((res) => {
        //     const result = res.data;
        //     const { name, html_url } = result.items[0];
        //     this.repoName = name;
        //     this.repoUrl = html_url;
        //   })
        //   .catch((error) => {
        //     alert("请求出错");
        //   });
    
        axios
          .get("https://api.github.com/search/repositories", {
            params: {
              q: "q",
              sort: "stars",
            },
          })
          .then((res) => {
            const result = res.data;
            const { name, html_url } = result.items[0];
            this.repoName = name;
            this.repoUrl = html_url;
          })
          .catch((error) => {
            alert("请求出错");
          });
      },
    };
    </script>
    
    <style>
    </style>
    ```

  - 调用`npm run dev`命令

## 补充vue-resource的使用

- 在线文档

  - `https://github.com/pagekit/vue-resource/blob/develop/docs/http.md`

- ```javascript
  // 引入模块
  import VueResource from 'vue-resource'
  // 使用插件
  Vue.use(VueResource)
  
  // 通过vue/组件对象发送ajax请求
  this.$http.get('/someUrl').then((response) => {
    // success callback
    console.log(response.data) //返回结果数据
  }, (response) => {
    // error callback
    console.log(response.statusText) //错误信息
  })
  ```

- 测试接口

  - > 接口1: [https://api.github.com/search/repositories?q=v&sort=stars](https://api.github.com/search/repositories?q=${this.searchName}&sort=stars)
    >
    > 接口2: [https://api.github.com/search/users?q=a](https://api.github.com/search/users?q=${value})

#
