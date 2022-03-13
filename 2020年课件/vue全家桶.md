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