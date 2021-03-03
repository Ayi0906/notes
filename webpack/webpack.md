[Toc]

## 17_优化配置介绍
### webpack性能优化
#### 开发环境性能优化
- 优化打包构建速度
- 优化调试功能

#### 生产环境性能优化
- 优化打包构建速度
- 优化代码运行的性能

## 18_HMR
- 生产环境不使用HMR
- 比如：调用`npx webpack-dev-server`，然后修改css文件保存，会发现js等其它文件也与css文件一同被再次打包，会花费大量时间。
- HMR：hot module replacement 热模块替换/模块热替换
  - 作用：一个模块发生变化，只会重新打包这一个模块（而不是打包所有模块），极大提升构建速度
  - 使用：在devServer中写入`hot:true`
  ```
  devServer:{
      // some code
      hot:true 
      // 开启HMR功能，当修改了webpack配置，需要重启
  }
  ```
  - 样式文件：可以使用HMR功能：因为style-loader内部实现了
      - 发现问题：开启HMR后，修改css文件后html没有任何改变
  - js文件：js文件默认没有HMR功能的
    - 修改js代码：添加支持HMR功能的代码
    ```
    // 写在index.js的末尾
    // HMR功能对js的处理，只能处理非入口js文件的其它文件
    if(module.hot){
        module.hot.accept('./print.js',function(){
            // 方法会监听 print.js的变化，一旦发生变化，其它模块不会重新打包构建
            print();
        })
    }
    ```
  - html文件：更改文档后（比如修改p内的语句）并没有变化。同时导致问题：html文件不能了更新了
    - 解决：修改entry入口，将hrml文件引入
    ```
    entry:['./src/js/index.js','./src/html/index.html']
    // 之后还是使用不了HMR功能，修改html文件还是全部重新打包
    ```
- html文件不需要做HMR功能，因为只有它一个文件

## 19_source-map
- 一种提供源代码到构建后代码映射的技术（如果构建后代码出错了，通过映射可以追踪到源代码）
```
devtool:'source-map'
```
  - 会在dist/js目录下生成'bundle.js.map'文件，形成了bundle.js与源代码的映射关系

- 如果js文件出现错误，那么在控制台里可以看到错误信息和对应的js文件的错误位置，比如'change.js:15'链接，点击链接可以直接跳转到change.js的源代码的第15行。
### source-map的几个参数
- [inline- |hidden- |eval- ][nosources- ][cheap- [module- ]]source-map

### inline-source-map / hidden-source-map / eval-source-map 的区别
- inline-source-map只生成一个内联source-map
- eval-source-map每一个文件都生产对应的source-map文件，都在eval中
- 内联和外部的区别
  - 内联构建速度更快
#### inline-source-map 内联
- 打包后不会额外生成一个'bundle.js.map'文件，而是在'bundle.js'文件中内联写入映射关系

- 和source-map一样，如果js文件出错，那么在控制台显示错误信息，'change.js:15'链接，点击链接跳转到change.js源代码错误位置

####  hidden-source-map 外部
- 它会生成'bundle.js.map'文件
- change.js文件出错，它会报告出错信息，但是报告位置是'bundle.js:12424',点击链接跳转到bundle.js的出错位置(12424行)，同时在浏览器的`Sources`选项中无法查看源代码，只能查看到打包后的dist的文件，但是dist内也无法查看`media`文件夹
- 目的：为了隐藏源代码


#### eval-source-map 内联
- 每一个js文件后面都会追加一个source-map文件
- 同inline-source-map一样，区别在于显示的错误链接为`change.js?518f:12`，多了一个hash值，点击它依然会跳转到chang.js源文件的出错位置。同样可以直接查看打包前的文件。

### nosources-source-map 外部
- change.js文件报错：显示错误信息和错误位置`change.js:12`
- 点击无法显示源文件，甚至不显示`bundle.js`的错误位置，也无法在浏览器的`Sources`中查看源文件
- 目的：为了隐藏源代码，这种方法比hidden-source-map隐藏地更彻底。不需要调试，彻底隐藏所有代码。

### cheap-source-map 外部
- 显示报错信息和源代码报错位置，但是点击显示的是源代码的一整行的报错。相比较source-map只报错具体位置。（有点智障，如果js被压缩那么很难分清哪里才是出错的地方）
- 可以在`Sources`中直接查看源代码

### cheap-module-source-map 外部
- 和cheap-source-map一样
- module会将loader的source map加入

### 生产环境和开发环境对于source-map的不同选择
- 开发环境：速度快，调试友好（即可以直接跳转到源文件出错位置）
- 生产环境：代码是否需要隐藏？是否需要调试？分别对应`hidden-source-map`和`nosources-source-map `
  - 内联会让体积变大，所有内联方式都要排除

- 速度快
    eval>inline>cheap
    eval-cheap-source-map
    eval-source-map
- 调试友好
    source-map
    cheap-module-source-map
    cheap-source-map
- eval-source-map调试最友好 / eval-cheap-module-source-map速度最快
- 很多脚手架默认使用eval-source-map

### 总结
- inline/hiddent/eval - nosources/cheap/cheap-module - source-map