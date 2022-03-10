[Toc]

## .vue单文件的stylus预编译移动端适配

### 1.将stylus预编译文件写在.vue文件内的`<style>`标签内

- ```javascript
  <style lang="">
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