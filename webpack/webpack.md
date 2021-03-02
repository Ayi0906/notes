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

