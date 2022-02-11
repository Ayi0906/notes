[Toc]
## 基本认识
### 五个核心概念
- Entry：入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。
- Entry：入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。
- Loader：loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只能解析 JavaScript）。
- Plugins：插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量等。
- Mode：模式，有生产模式production和开发模式development

### 理解Loader
- Webpack 本身只能加载JS/JSON模块，如果要加载其他类型的文件(模块)，就需要使用对应的loader 进行转换/加载
- Loader 本身也是运行在 node.js 环境中的 JavaScript 模块
- 它本身是一个函数，接受源文件作为参数，返回转换的结果
- loader 一般以 xxx-loader 的方式命名，xxx 代表了这个 loader 要做的转换功能，比如 json-loader。

### 理解Plugins
- 插件可以完成一些loader不能完成的功能。
- 插件的使用一般是在 webpack 的配置信息 plugins 选项中指定。

### 理解Plugins
- 插件可以完成一些loader不能完成的功能。
- 插件的使用一般是在 webpack 的配置信息 plugins 选项中指定。

## 初始化
### 初始化项目
- `npm init -y` 将当前项目初始化
### 本地和全局安装webpack
- `npm i webpack webpack-cli -d`
- `npm i webpack webpack-cli -g`

### 构建基本项目结构
```
|-项目
  |-src
    |-js
      |-index.js // 创建入口文件
    |-css
    |-less
    |-media
      |-img
      |-audio
      |-video
      |-icon
      |-font
  |-index.html // 创建主页面
  |-webpack.config.js
  |-package.json
  |-package-lock.json
  
```

## 运行指令
### 不使用webpack.config.js进行打包
- 开发配置指令：`webpack ./src/js/index.js -o ./dist/js --mode=development`
  - webpack能够编译打包js和json文件，并且能将es6的模块化语法转换成浏览器能识别的语法

- 生产配置指令:`webpack ./src/js/index.js -o ./dist/js --mode=production`
  - 在开发配置功能上加上一个压缩代码


### 总结
- webpack能够编译打包js和json文件, 能将es6的模块化语法转换成浏览器能识别的语法, 能压缩代码
- 不能编译打包css、img等文件, 不能将js的es6基本语法转化为es5以下语法

### 使用webpack.config.js进行打包
#### 配置webpack.config.js
```javascript
const {resolve} = require('path')
module.exports = {
    entry: { // 入口文件
        main: './src/js/index.js'
    },
    output: { // 出口
        filename: './js/bundle.js', // 输出文件名
        path: resolve(__dirname, 'dist') // 输出文件路径配置
    },
    mode:'development'
}
```
- 运行指令: webpack

## 1.打包css文件到html中的\<style\>标签中
- 在src/css目录中写入css文件
- 在index.js中import引入css文件
### 下载依赖
`npm i css-loader style-loader -d`
### 配置webpack.config.js
```javascript
const {resolve} = require('path')
module.exports = {
    entry: { // 入口文件
        main: './src/js/index.js'
    },
    output: { // 出口
        filename: './js/bundle.js', // 输出文件名
        path: resolve(__dirname, 'dist') // 输出文件路径配置
    },
    mode: 'development',
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader']
            }
        ]
    }
}
```
- 调用`webpack`命令, css文件被打包进bundle.js文件中, index.html引入bundle.js文件, bundle.js会在DOM中创建\<style\>标签并写入样式.

## 2.打包less文件,与css文件一起写入html中的\<style\>标签中
- 在src/less中写入less文件
- 在index.js中i孟婆汤引入less文件

### 下载依赖
`npm i less-loader -d`
### 配置webpack.config.js
```javascript
const {resolve} = require('path')
module.exports = {
    entry: { // 入口文件
        main: './src/js/index.js'
    },
    output: { // 出口
        filename: './js/bundle.js', // 输出文件名
        path: resolve(__dirname, 'dist') // 输出文件路径配置
    },
    mode: 'development',
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader']
            },
            {
                test: /\.less$/,
                use: ['style-loader', 'css-loader', 'less-loader']
            }
        ]
    }
}
```
- 调用`webpack`命令, css文件和less文件被打包进bundle.js文件中, index.html引入bundle.js文件, bundle.js会在DOM中创建\<style\>标签并写入样式.

## 3.提取css/less文件单独成文件
### 下载依赖
`npm i mini-css-extract-plugin -d`
### 配置webpack.config.js
```javascript
const {resolve} = require('path')
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
module.exports = {
    entry: { // 入口文件
        main: './src/js/index.js'
    },
    output: { // 出口
        filename: './js/bundle.js', // 输出文件名
        path: resolve(__dirname, 'dist') // 输出文件路径配置
    },
    mode: 'development',
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [{loader: MiniCssExtractPlugin.loader}, 'css-loader']
            },
            {
                test: /\.less$/,
                use: [{loader: MiniCssExtractPlugin.loader}, 'css-loader', 'less-loader']
            }
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({
            filename: 'css/[contenthash:10].css' // 生成的文件以10位hash值为文件名
        })
    ]
}
```
- 调用`webpack`命令, css和less文件都被打包进了dist/css目录中,需要在html中以`<link>`标签引入

## 4.css兼容性处理
### 安装依赖
`npm install postcss-loader postcss-flexbugs-fixes postcss-preset-env postcss-normalize autoprefixer postcss-nested --save-dev`

### 在webpack.config.js同级目录写一个postcss.config.js
```javascript
module.exports = {
    ident: 'postcss',
    plugins: [
        require('postcss-flexbugs-fixes'),
        require('postcss-preset-env')({
            autoprefixer: {
                flexbox: 'no-2009',
            },
            stage: 3,
        }),
        require('postcss-normalize')(),
        require('postcss-nested')
    ],
    sourceMap: true
}
```

### 配置webpack.config.js
```javascript
const {resolve} = require('path')
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
module.exports = {
    entry: { // 入口文件
        main: './src/js/index.js'
    },
    output: { // 出口
        filename: './js/bundle.js', // 输出文件名
        path: resolve(__dirname, 'dist') // 输出文件路径配置
    },
    mode: 'development',
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    // 将所有css文件打包为一个文件
                    MiniCssExtractPlugin.loader,
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader' // css兼容性处理
                ]
            },
            {
                test: /\.less$/,
                use: [
                    // 将所有css文件打包为一个文件
                    MiniCssExtractPlugin.loader,
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader', // css兼容性处理
                    'less-loader' // less文件转为css文件
                ]
            }
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({
            // 输出到dist/css目录,且文件名为10位hash值
            filename: 'css/[contenthash:10].css'
        })
    ]
}
```
- postcss配置参考来源
  - **postcss-loader** `https://www.npmjs.com/package/postcss-loader`
  - **autoprefixer**:`https://github.com/postcss/autoprefixer#webpack`
  - **postcss**:`https://github.com/postcss/postcss/blob/main/docs/README-cn.md`
  - **postcss-preset-env**:`https://www.npmjs.com/package/postcss-preset-env`
### 配置package.json
```
  "browserslist": [
    "last 1 version",
    "> 1%",
    "IE 10"
  ]
```
配置参考来源:`https://github.com/browserslist/browserslist`

### 备注
- flex无法兼容到IE9以及以下浏览器


## 5.压缩css文件
### 安装依赖
`npm install css-minimizer-webpack-plugin --save-dev`
- ~~webpack5要求以上插件,如果是webpack4,安装`Optimze-css-assets-webpack-plugin`~~

### 配置webpack.config.js
```javascript
const {resolve} = require('path')
// 用于整合css为同一个文件
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
// 用于压缩css文件
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin")
module.exports = {
    entry: { // 入口文件
        main: './src/js/index.js'
    },
    output: { // 出口
        filename: './js/bundle.js', // 输出文件名
        path: resolve(__dirname, 'dist') // 输出文件路径配置
    },
    mode: 'development',
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    // 将所有css文件打包为一个文件
                    MiniCssExtractPlugin.loader,
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader' // css兼容性处理
                ]
            },
            {
                test: /\.less$/,
                use: [
                    // 将所有css文件打包为一个文件
                    MiniCssExtractPlugin.loader,
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader', // css兼容性处理
                    'less-loader' // less文件转为css文件
                ]
            }
        ]
    },
    optimization: {
        /* 取消以下配置的注释, 可以在开发环境下压缩css文件 */
        // minimize: true,

        /* 以下配置保证在生产环境下能够压缩css文件 */
        minimizer: [
            // For webpack@5 you can use the `...` syntax to extend existing minimizers (i.e. `terser-webpack-plugin`), uncomment the next line
            // `...`,
            new CssMinimizerPlugin()
        ],
    },
    plugins: [
        new MiniCssExtractPlugin({
            // 输出到dist/css目录,且文件名为10位hash值
            filename: 'css/[contenthash:10].css'
        })
    ]
}
```

### postcss.config.js保留原配置(详见兼容css)
### package.json保留原配置(详见兼容css)
### 配置参考来源
- `https://www.npmjs.com/package/css-minimizer-webpack-plugin`

## 6.js语法检查
### 安装依赖
- ~~`npm install eslint-loader eslint --save-dev`~~(webpack4)
- `npm install eslint eslint-webpack-plugin --save-dev`
  - 最新提示esilnt-loader已被弃用,使用`eslint-webpack-plugin`

### 配置webpack.config.js
```javascript
const {resolve} = require('path')
// 用于整合css为同一个文件
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
// 用于压缩css文件
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin")
// 用于js语法检查
const ESLintPlugin = require('eslint-webpack-plugin')
module.exports = {
    entry: { // 入口文件
        main: './src/js/index.js'
    },
    output: { // 出口
        filename: './js/bundle.js', // 输出文件名
        path: resolve(__dirname, 'dist') // 输出文件路径配置
    },
    mode: 'development',
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    // 将所有css文件打包为一个文件
                    MiniCssExtractPlugin.loader,
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader' // css兼容性处理
                ]
            },
            {
                test: /\.less$/,
                use: [
                    // 将所有css文件打包为一个文件
                    MiniCssExtractPlugin.loader,
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader', // css兼容性处理
                    'less-loader' // less文件转为css文件
                ]
            }
        ]
    },
    optimization: {
        /* 取消以下配置的注释, 可以在开发环境下压缩css文件 */
        // minimize: true,

        /* 以下配置保证在生产环境下能够压缩css文件 */
        minimizer: [
            // For webpack@5 you can use the `...` syntax to extend existing minimizers (i.e. `terser-webpack-plugin`), uncomment the next line
            // `...`,
            new CssMinimizerPlugin()
        ],
    },
    plugins: [
        new MiniCssExtractPlugin({
            // 输出到dist/css目录,且文件名为10位hash值
            filename: 'css/[contenthash:10].css'
        }),
        new ESLintPlugin({
            context: resolve(__dirname), //指定文件根目录，类型为字符串
            extensions: 'js', // 指定需要检查的扩展名 ,默认js
            exclude: '/node_modules/', // 排除node_modules文件夹, 配置里可以只保留这一项
            fix: false, // 启用 ESLint 自动修复特性。 小心: 该选项会修改源文件。
            quiet: false, // 设置为 true 后，仅处理和报告错误，忽略警告

        })
    ]
}
```
- 测试:在index.js或其它js文件中写入一个错误, 它就会报错
### postcss.config.js保留原配置(详见兼容css)
### package.json保留原配置(详见兼容css)

### 官网
- `https://webpack.docschina.org/plugins/eslint-webpack-plugin/`
- `https://www.npmjs.com/package/eslint-webpack-plugin`
#### 额外补充eslint的官网
- `https://eslint.bootcss.com/docs/user-guide/getting-started`


## 7.js兼容性处理
### 安装依赖
- `npm i babel-loader @babel/core @babel/preset-env @babel/polyfill core-js -d`

### 配置webpack.config.js
```javascript
const {resolve} = require('path')
// 用于整合css为同一个文件
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
// 用于压缩css文件
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin")
// 用于js语法检查
const ESLintPlugin = require('eslint-webpack-plugin')
module.exports = {
    entry: { // 入口文件
        main: './src/js/index.js'
    },
    output: { // 出口
        filename: './js/bundle.js', // 输出文件名
        path: resolve(__dirname, 'dist') // 输出文件路径配置
    },
    mode: 'development',
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    // 将所有css文件打包为一个文件
                    MiniCssExtractPlugin.loader,
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader' // css兼容性处理
                ]
            },
            {
                test: /\.less$/,
                use: [
                    // 将所有css文件打包为一个文件
                    MiniCssExtractPlugin.loader,
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader', // css兼容性处理
                    'less-loader' // less文件转为css文件
                ]
            },
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: [
                            [
                                '@babel/preset-env',
                                {
                                    useBuiltIns: 'usage',  // 按需引入需要使用polyfill
                                    corejs: { version: 3 }, // 解决warn
                                    targets: { // 指定兼容性处理哪些浏览器
                                        "chrome": "58",
                                        "ie": "9",
                                    }
                                }
                            ]
                        ],
                        cacheDirectory: true, // 开启babel缓存
                    }
                }
            }
        ]
    },
    optimization: {
        /* 取消以下配置的注释, 可以在开发环境下压缩css文件 */
        // minimize: true,

        /* 以下配置保证在生产环境下能够压缩css文件 */
        minimizer: [
            // For webpack@5 you can use the `...` syntax to extend existing minimizers (i.e. `terser-webpack-plugin`), uncomment the next line
            // `...`,
            new CssMinimizerPlugin()
        ],
    },
    plugins: [
        new MiniCssExtractPlugin({
            // 输出到dist/css目录,且文件名为10位hash值
            filename: 'css/[contenthash:10].css'
        }),
        new ESLintPlugin({
            context: resolve(__dirname), //指定文件根目录，类型为字符串
            extensions: 'js', // 指定需要检查的扩展名 ,默认js
            exclude: '/node_modules/', // 排除node_modules文件夹, 配置里可以只保留这一项
            fix: false, // 启用 ESLint 自动修复特性。 小心: 该选项会修改源文件。
            quiet: false, // 设置为 true 后，仅处理和报告错误，忽略警告

        })
    ]
}
```
### postcss.config.js保留原配置(详见兼容css)
### package.json保留原配置(详见兼容css)

### 官网
- `https://babel.docschina.org/docs/en/babel-preset-env/`
- babel的配置多且杂

## 8.打包样式中的图片资源
### 安装依赖
- 不安装任何依赖
### webpack.config.js更新
```javascript
            /* 打包样式文件中的图片资源 */
            {
                test: /\.(jpe?g|png|gif)$/i,
                type: "asset",
                generator: {
                    filename: 'media/img/[hash:8][ext]'
                }
            }
```

### webpack.config.js完整
```javascript
const {resolve} = require('path')
// 用于整合css为同一个文件
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
// 用于压缩css文件
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin")
// 用于js语法检查
const ESLintPlugin = require('eslint-webpack-plugin')
module.exports = {
    entry: { // 入口文件
        main: './src/js/index.js'
    },
    output: { // 出口
        filename: './js/bundle.js', // 输出文件名
        path: resolve(__dirname, 'dist'),// 输出文件路径配置
        clean: true // 每次打包前都清空dist文件夹
    },
    mode: 'production',
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    // css文件进行兼容性处理, 将所有css文件打包为一个文件
                    {
                        loader: MiniCssExtractPlugin.loader
                    },
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader' // css兼容性处理
                ]
            },
            /* less文件转css文件, 进行兼容性处理, 与css文件打包为同一个文件 */
            {
                test: /\.less$/,
                use: [
                    // 将所有css文件打包为一个文件
                    {
                        loader: MiniCssExtractPlugin.loader
                    },
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader', // css兼容性处理
                    'less-loader' // less文件转为css文件
                ]
            },
            /* js兼容性处理 */
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: [
                            [
                                '@babel/preset-env',
                                {
                                    useBuiltIns: 'usage',  // 按需引入需要使用polyfill
                                    corejs: {version: 3}, // 解决warn
                                    targets: { // 指定兼容性处理哪些浏览器
                                        "chrome": "58",
                                        "ie": "9",
                                    }
                                }
                            ]
                        ],
                        cacheDirectory: true, // 开启babel缓存
                    }
                }
            },
            /* 打包样式文件中的图片资源 */
            {
                test: /\.(jpe?g|png|gif)$/i,
                type: "asset",
                generator: {
                    filename: 'media/img/[hash:8][ext]' // 放入dist/media/img 目录中
                },
                parser: {
                    dataUrlCondition: {
                        maxSize: 10 * 1024 // 超过100kb不转 base64
                    }
                }
            }
        ]
    },
    optimization: {
        /* 取消以下配置的注释, 可以在开发环境下压缩css文件 */
        // minimize: true,

        /* 以下配置保证在生产环境下能够压缩css文件 */
        minimizer: [
            // For webpack@5 you can use the `...` syntax to extend existing minimizers (i.e. `terser-webpack-plugin`), uncomment the next line
            // `...`,
            new CssMinimizerPlugin()
        ],
    },
    plugins: [
        new MiniCssExtractPlugin({
            // 输出到dist/css目录,且文件名为10位hash值
            filename: 'css/[hash:10].css'
        }),
        new ESLintPlugin({
            context: resolve(__dirname), //指定文件根目录，类型为字符串
            extensions: 'js', // 指定需要检查的扩展名 ,默认js
            exclude: '/node_modules/', // 排除node_modules文件夹, 配置里可以只保留这一项
            fix: false, // 启用 ESLint 自动修复特性。 小心: 该选项会修改源文件。
            quiet: false, // 设置为 true 后，仅处理和报告错误，忽略警告
        })
    ]
}
```

### js文件中新建的`<img>`
- js文件操作DOM引用外部资源的`<img>`
```javascript
    let oImg = document.createElement("img")
    oImg.width = '100'
    oImg.height = '150'
    oImg.src = 'https://t7.baidu.com/it/u=376303577,3502948048&fm=193&f=GIF'
    document.body.appendChild(oImg)
```
- js文件操作DOM引用静态资源的`<img>`
```javascript
    // 需要先引入图片
    import imgSrc from '../media/img/73b74568.jpg'

    let oImg = document.createElement('img')
    oImg.src = imgSrc
    oImg.width = '200'
    oImg.height = '300'
    document.body.appendChild(oImg)
```
### 说明
- 在webpack4中广泛使用url-loader与file-loader,但是webpack5不需要使用


## 9.创建html文件副本并自动引入打包好的css文件与js文件和html中的引用图片资源
- 之前一直都是将css,js等打包好之后再逐一引入,现在要求在dist目录中创建index.html副本并且自动引入打包好的资源文件
### 安装依赖
- `npm i html-loader html-webpack-plugin -d`

### webpack.config.js新增
```javascript
// 用于打包html文件
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports={
    // some code
    module:{
        rules:[
            /* 打包html中引用的资源 */
            {
                test: /\.html$/,
                use: {
                    loader: 'html-loader'
                }
            }
        ]
    },
    plugins:[
        new HtmlWebpackPlugin({
            template: './index.html'
        })
    ]
}
``` 
- 注意: 能够打包html中的`<img src>`引入的图片是因为前面已经写好了图片打包逻辑, 通过在配置中继续写入对字体,音视频的打包支持, 也可以打包html中的`<video src>`和`<audio src>`

### webpack.config.js全部配置
```javascript
const {resolve} = require('path')
// 用于整合css为同一个文件
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
// 用于压缩css文件
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin")
// 用于js语法检查
const ESLintPlugin = require('eslint-webpack-plugin')
// 用于打包html文件
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    entry: { // 入口文件
        main: './src/js/index.js'
    },
    output: { // 出口
        filename: './js/bundle.js', // 输出文件名
        path: resolve(__dirname, 'dist'),// 输出文件路径配置
        clean: true // 每次打包前都清空dist文件夹
    },
    mode: 'development',
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    // css文件进行兼容性处理, 将所有css文件打包为一个文件
                    {
                        loader: MiniCssExtractPlugin.loader
                    },
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader' // css兼容性处理
                ]
            },
            /* less文件转css文件, 进行兼容性处理, 与css文件打包为同一个文件 */
            {
                test: /\.less$/,
                use: [
                    // 将所有css文件打包为一个文件
                    {
                        loader: MiniCssExtractPlugin.loader
                    },
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader', // css兼容性处理
                    'less-loader' // less文件转为css文件
                ]
            },
            /* js兼容性处理 */
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: [
                            [
                                '@babel/preset-env',
                                {
                                    useBuiltIns: 'usage',  // 按需引入需要使用polyfill
                                    corejs: {version: 3}, // 解决warn
                                    targets: { // 指定兼容性处理哪些浏览器
                                        "chrome": "58",
                                        "ie": "10",
                                    }
                                }
                            ]
                        ],
                        cacheDirectory: true, // 开启babel缓存
                    }
                }
            },
            /* 打包样式文件中的图片资源 */
            {
                test: /\.(jpe?g|png|gif)$/i,
                type: "asset",
                generator: {
                    filename: 'media/img/[hash:8][ext]' // 放入dist/media/img 目录中
                },
                parser: {
                    dataUrlCondition: {
                        maxSize: 10 * 1024 // 超过100kb不转 base64
                    }
                }
            },
            /* 打包html中引用的资源 */
            {
                test: /\.html$/,
                use: {
                    loader: 'html-loader'
                }
            }
        ]
    },
    optimization: {
        /* 取消以下配置的注释, 可以在开发环境下压缩css文件 */
        // minimize: true,

        /* 以下配置保证在生产环境下能够压缩css文件 */
        minimizer: [
            // For webpack@5 you can use the `...` syntax to extend existing minimizers (i.e. `terser-webpack-plugin`), uncomment the next line
            // `...`,
            // css压缩
            new CssMinimizerPlugin(),
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({
            // 输出到dist/css目录,且文件名为10位hash值
            filename: 'css/[hash:10].css'
        }),
        new ESLintPlugin({
            context: resolve(__dirname), //指定文件根目录，类型为字符串
            extensions: 'js', // 指定需要检查的扩展名 ,默认js
            exclude: '/node_modules/', // 排除node_modules文件夹, 配置里可以只保留这一项
            fix: false, // 启用 ESLint 自动修复特性。 小心: 该选项会修改源文件。
            quiet: false, // 设置为 true 后，仅处理和报告错误，忽略警告
        }),
        new HtmlWebpackPlugin({
            template: './index.html'
        })
    ]
}
```

## 10.对于音视频文件的打包
### 安装依赖
- 音视频与图片资源一样,并不需要额外引入包

### webpack.config.js新增
```javascript
            /* 打包音频资源 */
            {
                test: /\.mp3$/i,
                type: "asset",
                generator: {
                    filename: 'media/audio/[hash:8][ext]' // 放入dist/media/audio 目录中
                }
            },

            /* 打包视频资源 */
            {
                test: /\.mp4$/i,
                type: "asset",
                generator: {
                    filename: 'media/video/[hash:8][ext]' // 放入dist/media/video 目录中
                }
            },
```
### webpack.config.js完整
```javascript
const {resolve} = require('path')
// 用于整合css为同一个文件
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
// 用于压缩css文件
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin")
// 用于js语法检查
const ESLintPlugin = require('eslint-webpack-plugin')
// 用于打包html文件
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    entry: { // 入口文件
        main: './src/js/index.js'
    },
    output: { // 出口
        filename: './js/bundle.js', // 输出文件名
        path: resolve(__dirname, 'dist'),// 输出文件路径配置
        clean: true // 每次打包前都清空dist文件夹
    },
    mode: 'development',
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    // css文件进行兼容性处理, 将所有css文件打包为一个文件
                    {
                        loader: MiniCssExtractPlugin.loader
                    },
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader' // css兼容性处理
                ]
            },
            /* less文件转css文件, 进行兼容性处理, 与css文件打包为同一个文件 */
            {
                test: /\.less$/,
                use: [
                    // 将所有css文件打包为一个文件
                    {
                        loader: MiniCssExtractPlugin.loader
                    },
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader', // css兼容性处理
                    'less-loader' // less文件转为css文件
                ]
            },
            /* js兼容性处理 */
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: [
                            [
                                '@babel/preset-env',
                                {
                                    useBuiltIns: 'usage',  // 按需引入需要使用polyfill
                                    corejs: {version: 3}, // 解决warn
                                    targets: { // 指定兼容性处理哪些浏览器
                                        "chrome": "58",
                                        "ie": "10",
                                    }
                                }
                            ]
                        ],
                        cacheDirectory: true, // 开启babel缓存
                    }
                }
            },
            /* 打包样式文件中的图片资源 */
            {
                test: /\.(jpe?g|png|gif)$/i,
                type: "asset",
                generator: {
                    filename: 'media/img/[hash:8][ext]' // 放入dist/media/img 目录中
                },
                parser: {
                    dataUrlCondition: {
                        maxSize: 10 * 1024 // 超过100kb不转 base64
                    }
                }
            },
            /* 打包音频资源 */
            {
                test: /\.mp3$/i,
                type: "asset",
                generator: {
                    filename: 'media/audio/[hash:8][ext]' // 放入dist/media/audio 目录中
                }
            },

            /* 打包视频资源 */
            {
                test: /\.mp4$/i,
                type: "asset",
                generator: {
                    filename: 'media/video/[hash:8][ext]' // 放入dist/media/video 目录中
                }
            },

            /* 打包html中引用的资源 */
            {
                test: /\.html$/,
                use: {
                    loader: 'html-loader'
                }
            }
        ]
    },
    optimization: {
        /* 取消以下配置的注释, 可以在开发环境下压缩css文件 */
        // minimize: true,

        /* 以下配置保证在生产环境下能够压缩css文件 */
        minimizer: [
            // For webpack@5 you can use the `...` syntax to extend existing minimizers (i.e. `terser-webpack-plugin`), uncomment the next line
            // `...`,
            // css压缩
            new CssMinimizerPlugin(),
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({
            // 输出到dist/css目录,且文件名为10位hash值
            filename: 'css/[hash:10].css'
        }),
        new ESLintPlugin({
            context: resolve(__dirname), //指定文件根目录，类型为字符串
            extensions: 'js', // 指定需要检查的扩展名 ,默认js
            exclude: '/node_modules/', // 排除node_modules文件夹, 配置里可以只保留这一项
            fix: false, // 启用 ESLint 自动修复特性。 小心: 该选项会修改源文件。
            quiet: false, // 设置为 true 后，仅处理和报告错误，忽略警告
        }),
        new HtmlWebpackPlugin({
            template: './index.html'
        })
    ]
}
```

## 11.对于字体文件的打包
### 安装依赖
- 字体与图片资源一样,并不需要额外引入包

### webpack.config.js新增
```javascript
            /* 打包字体文件 */
            {
                test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/i,
                type: "asset",
                generator: {
                    filename: 'media/font/[hash:8][ext]' // 放入dist/media/font 目录中
                }
            },
```
### webpack.config.js完整
```javascript
const {resolve} = require('path')
// 用于整合css为同一个文件
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
// 用于压缩css文件
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin")
// 用于js语法检查
const ESLintPlugin = require('eslint-webpack-plugin')
// 用于打包html文件
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    entry: { // 入口文件
        main: './src/js/index.js'
    },
    output: { // 出口
        filename: './js/bundle.js', // 输出文件名
        path: resolve(__dirname, 'dist'),// 输出文件路径配置
        clean: true // 每次打包前都清空dist文件夹
    },
    mode: 'development',
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    // css文件进行兼容性处理, 将所有css文件打包为一个文件
                    {
                        loader: MiniCssExtractPlugin.loader
                    },
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader' // css兼容性处理
                ]
            },
            /* less文件转css文件, 进行兼容性处理, 与css文件打包为同一个文件 */
            {
                test: /\.less$/,
                use: [
                    // 将所有css文件打包为一个文件
                    {
                        loader: MiniCssExtractPlugin.loader
                    },
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader', // css兼容性处理
                    'less-loader' // less文件转为css文件
                ]
            },
            /* js兼容性处理 */
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: [
                            [
                                '@babel/preset-env',
                                {
                                    useBuiltIns: 'usage',  // 按需引入需要使用polyfill
                                    corejs: {version: 3}, // 解决warn
                                    targets: { // 指定兼容性处理哪些浏览器
                                        "chrome": "58",
                                        "ie": "10",
                                    }
                                }
                            ]
                        ],
                        cacheDirectory: true, // 开启babel缓存
                    }
                }
            },
            /* 打包样式文件中的图片资源 */
            {
                test: /\.(jpe?g|png|gif)$/i,
                type: "asset",
                generator: {
                    filename: 'media/img/[hash:8][ext]' // 放入dist/media/img 目录中
                },
                parser: {
                    dataUrlCondition: {
                        maxSize: 10 * 1024 // 超过100kb不转 base64
                    }
                }
            },
            /* 打包音频资源 */
            {
                test: /\.mp3$/i,
                type: "asset",
                generator: {
                    filename: 'media/audio/[hash:8][ext]' // 放入dist/media/audio 目录中
                }
            },

            /* 打包视频资源 */
            {
                test: /\.mp4$/i,
                type: "asset",
                generator: {
                    filename: 'media/video/[hash:8][ext]' // 放入dist/media/video 目录中
                }
            },

            /* 打包字体文件 */
            {
                test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/i,
                type: "asset",
                generator: {
                    filename: 'media/font/[hash:8][ext]' // 放入dist/media/font 目录中
                }
            },

            /* 打包html中引用的资源 */
            {
                test: /\.html$/,
                use: {
                    loader: 'html-loader'
                }
            }
        ]
    },
    optimization: {
        /* 取消以下配置的注释, 可以在开发环境下压缩css文件 */
        // minimize: true,

        /* 以下配置保证在生产环境下能够压缩css文件 */
        minimizer: [
            // For webpack@5 you can use the `...` syntax to extend existing minimizers (i.e. `terser-webpack-plugin`), uncomment the next line
            // `...`,
            // css压缩
            new CssMinimizerPlugin(),
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({
            // 输出到dist/css目录,且文件名为10位hash值
            filename: 'css/[hash:10].css'
        }),
        new ESLintPlugin({
            context: resolve(__dirname), //指定文件根目录，类型为字符串
            extensions: 'js', // 指定需要检查的扩展名 ,默认js
            exclude: '/node_modules/', // 排除node_modules文件夹, 配置里可以只保留这一项
            fix: false, // 启用 ESLint 自动修复特性。 小心: 该选项会修改源文件。
            quiet: false, // 设置为 true 后，仅处理和报告错误，忽略警告
        }),
        new HtmlWebpackPlugin({
            template: './index.html'
        })
    ]
}
```
## 12.对于icon-font字体的打包
- 开发阶段在index.html中引入iconfont.css文件, 但是在打包阶段是不允许在html中外链任何文件的,删去`<link>`引入的文件,在入口文件`index.js`中引入iconfont.css文件

### index.js中引入样式文件
```javascript
/* 引入iconfont的样式文件 */
import '../media/iconfont/font_eu399ydqqyk/iconfont.css'
```
- webpack.config.js文件与 11 一致

## 13.自动编译打包运行
### 安装依赖
- `npm i webpack-dev-server -d`

### webpack.config.js新增
```javascript
    /* dev-serve 自动化编译 */
    devServer: {
        hot: true,
        static: { //托管静态资源文件
            directory: resolve(__dirname, "/src"),
        },
        compress: true, //是否启动压缩 gzip
        port: 8080, // 端口号
        open: true,  // 是否自动打开浏览器
        client: { //在浏览器端打印编译进度
            progress: true,
        },
    },
```
### 解决修改html文件不更新的问题
- index.js入口文件中引入html文件,否则对html文件进行更改不会自动更新
```javascript
/* 引入index.html监听html文件的更新, 达到自动刷新的目的 */
import '../../index.html'
```
- 或者直接在`webpack.config`中entry写入:
```javascript
    entry: { // 入口文件
        main: ['./src/js/index.js','./index.html']
    },
```
- 以上两种方式都可以
### 修改MiniCssExtractPlugin下的打包后的css文件名
- 最大的问题在于dev-server无法更新index.html的资源链接,`<link>`和`<script>`都不行,所有通过hash命名的资源在更新后都会失效
- 需要将打包后的css文件命名为静态文件

```javascript
    plugins: [
        new MiniCssExtractPlugin({
            // 输出到dist/css目录,且文件名为10位hash值
            filename: './css/[name].css',
        }),
    ]
```
### `npx webpack-serve` 运行开发服务器
### 修改package.json文件
```json
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "serve": "webpack-serve"
  },
```
- 运行 `npm run serve`命令启用开发服务器

### webpack.config.js完整
```javascript
const {resolve} = require('path')
// 用于整合css为同一个文件
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
// 用于压缩css文件
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin")
// 用于js语法检查
const ESLintPlugin = require('eslint-webpack-plugin')
// 用于打包html文件
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    entry: { // 入口文件
        main: './src/js/index.js'
    },
    output: { // 出口
        filename: './js/bundle.js', // 输出文件名
        path: resolve(__dirname, 'dist'),// 输出文件路径配置
        clean: true // 每次打包前都清空dist文件夹
    },
    mode: 'development',
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    // css文件进行兼容性处理, 将所有css文件打包为一个文件
                    {
                        loader: MiniCssExtractPlugin.loader,
                    },
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader' // css兼容性处理
                ]
            },
            /* less文件转css文件, 进行兼容性处理, 与css文件打包为同一个文件 */
            {
                test: /\.less$/,
                use: [
                    // 将所有css文件打包为一个文件
                    {
                        loader: MiniCssExtractPlugin.loader,
                    },
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader', // css兼容性处理
                    'less-loader' // less文件转为css文件
                ]
            },
            /* js兼容性处理 */
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: [
                            [
                                '@babel/preset-env',
                                {
                                    useBuiltIns: 'usage',  // 按需引入需要使用polyfill
                                    corejs: {version: 3}, // 解决warn
                                    targets: { // 指定兼容性处理哪些浏览器
                                        "chrome": "58",
                                        "ie": "10",
                                    }
                                }
                            ]
                        ],
                        cacheDirectory: true, // 开启babel缓存
                    }
                }
            },
            /* 打包样式文件中的图片资源 */
            {
                test: /\.(jpe?g|png|gif)$/i,
                type: "asset",
                generator: {
                    filename: 'media/img/[hash:8][ext]' // 放入dist/media/img 目录中
                },
                parser: {
                    dataUrlCondition: {
                        maxSize: 10 * 1024 // 超过100kb不转 base64
                    }
                }
            },
            /* 打包音频资源 */
            {
                test: /\.mp3$/i,
                type: "asset",
                generator: {
                    filename: 'media/audio/[hash:8][ext]' // 放入dist/media/audio 目录中
                }
            },

            /* 打包视频资源 */
            {
                test: /\.mp4$/i,
                type: "asset",
                generator: {
                    filename: 'media/video/[hash:8][ext]' // 放入dist/media/video 目录中
                }
            },

            /* 打包字体文件 */
            {
                test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/i,
                type: "asset",
                generator: {
                    filename: 'media/font/[hash:8][ext]' // 放入dist/media/font 目录中
                }
            },

            /* 打包html中引用的资源 */
            {
                test: /\.html$/,
                use: {
                    loader: 'html-loader'
                }
            }
        ]
    },
    optimization: {
        /* 取消以下配置的注释, 可以在开发环境下压缩css文件 */
        minimize: true,

        /* 以下配置保证在生产环境下能够压缩css文件 */
        minimizer: [
            // For webpack@5 you can use the `...` syntax to extend existing minimizers (i.e. `terser-webpack-plugin`), uncomment the next line
            // `...`,
            // css压缩
            new CssMinimizerPlugin(),
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({
            // 输出到dist/css目录,且文件名为10位hash值
            filename: './css/[name].css',
        }),
        new ESLintPlugin({
            context: resolve(__dirname), //指定文件根目录，类型为字符串
            extensions: 'js', // 指定需要检查的扩展名 ,默认js
            exclude: '/node_modules/', // 排除node_modules文件夹, 配置里可以只保留这一项
            fix: false, // 启用 ESLint 自动修复特性。 小心: 该选项会修改源文件。
            quiet: false, // 设置为 true 后，仅处理和报告错误，忽略警告
        }),
        new HtmlWebpackPlugin({
            template: './index.html'
        })
    ],

    /* dev-serve 自动化编译 */
    devServer: {
        hot: true, // 热更新
        static: { //托管静态资源文件
            directory: resolve(__dirname, "/src"),
        },
        compress: true, //是否启动压缩 gzip
        port: 8080, // 端口号
        open: true,  // 是否自动打开浏览器
        client: { //在浏览器端打印编译进度
            progress: true,
        },
    },

}
```


## 14.HMR热模块替换

  - 添加 hot:true 上面已经添加了
  - 测试: 写一个input在html里面,键入字符, 然后更改css文件, 会发现样式改变后, 键入的字符并不受影响. 如果不写,就是整个页面全部刷新, 也就没有了字符.


## devtool映射技术
### production模式下无法压缩js代码
- 在webpack.config.js中添加如下代码

```javascript
    // 压缩js代码 , webpack5自带不需要安装
const TerserPlugin = require("terser-webpack-plugin");
module.exports={
    // some code
    optimization: {
        /* 取消以下配置的注释, 可以在开发环境下压缩css文件 */
        minimize: true,

        /* 以下配置保证在生产环境下能够压缩css文件 */
        minimizer: [
            // For webpack@5 you can use the `...` syntax to extend existing minimizers (i.e. `terser-webpack-plugin`), uncomment the next line
            // `...`,
            // css压缩
            new CssMinimizerPlugin(),
            /* 似乎使用了CSS压缩就会造成在生成模式下webpack无法压缩js代码, 于是使用这一内置插件用以压缩js代码 */
            /* 开发环境下请注释掉下面的代码 */
            new TerserPlugin({
                parallel: true, // 可省略，默认开启并行
                terserOptions: {
                    toplevel: true, // 最高级别，删除无用代码
                    ie8: true,
                    safari10: true,
                }
            })
        ]
    },
}
```

### 映射添加如下代码
```javascript
    devtool: process.env.NODE_ENV === 'production' ? 'eval-cheap-module-source-map' : 'inline-source-map', // 生产环境和开发环境的配置
```

### 完整wenpack.config.js代码
```javascript
const {resolve} = require('path')
// 用于整合css为同一个文件
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
// 用于压缩css文件
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin")
// 用于js语法检查
const ESLintPlugin = require('eslint-webpack-plugin')
// 用于打包html文件
const HtmlWebpackPlugin = require('html-webpack-plugin')
// 压缩js代码 , webpack5自带不需要安装
const TerserPlugin = require("terser-webpack-plugin");
module.exports = {
    entry: { // 入口文件
        main: ['./src/js/index.js', './index.html']
    },
    output: { // 出口
        filename: './js/bundle.js', // 输出文件名
        path: resolve(__dirname, 'dist'),// 输出文件路径配置
        clean: true // 每次打包前都清空dist文件夹
    },
    mode: 'development',
    module: {
        rules: [
            {
                test: /\.css$/,
                use: [
                    // css文件进行兼容性处理, 将所有css文件打包为一个文件
                    {
                        loader: MiniCssExtractPlugin.loader,
                    },
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader' // css兼容性处理
                ]
            },
            /* less文件转css文件, 进行兼容性处理, 与css文件打包为同一个文件 */
            {
                test: /\.less$/,
                use: [
                    // 将所有css文件打包为一个文件
                    {
                        loader: MiniCssExtractPlugin.loader,
                    },
                    {
                        loader: 'css-loader',
                        options: {
                            // importLoader配置借鉴于官网
                            importLoaders: 1,
                        }
                    },
                    'postcss-loader', // css兼容性处理
                    'less-loader' // less文件转为css文件
                ]
            },
            /* js兼容性处理 */
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: [
                            [
                                '@babel/preset-env',
                                {
                                    useBuiltIns: 'usage',  // 按需引入需要使用polyfill
                                    corejs: {version: 3}, // 解决warn
                                    targets: { // 指定兼容性处理哪些浏览器
                                        "chrome": "58",
                                        "ie": "10",
                                    }
                                }
                            ]
                        ],
                        cacheDirectory: true, // 开启babel缓存
                    }
                }
            },
            /* 打包样式文件中的图片资源 */
            {
                test: /\.(jpe?g|png|gif)$/i,
                type: "asset",
                generator: {
                    filename: 'media/img/[hash:8][ext]' // 放入dist/media/img 目录中
                },
                parser: {
                    dataUrlCondition: {
                        maxSize: 10 * 1024 // 超过100kb不转 base64
                    }
                }
            },
            /* 打包音频资源 */
            {
                test: /\.mp3$/i,
                type: "asset",
                generator: {
                    filename: 'media/audio/[hash:8][ext]' // 放入dist/media/audio 目录中
                }
            },

            /* 打包视频资源 */
            {
                test: /\.mp4$/i,
                type: "asset",
                generator: {
                    filename: 'media/video/[hash:8][ext]' // 放入dist/media/video 目录中
                }
            },

            /* 打包字体文件 */
            {
                test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/i,
                type: "asset",
                generator: {
                    filename: 'media/font/[hash:8][ext]' // 放入dist/media/font 目录中
                }
            },

            /* 打包html中引用的资源 */
            {
                test: /\.html$/,
                use: {
                    loader: 'html-loader'
                }
            }
        ]
    },
    optimization: {
        /* 取消以下配置的注释, 可以在开发环境下压缩css文件 */
        minimize: true,

        /* 以下配置保证在生产环境下能够压缩css文件 */
        minimizer: [
            // For webpack@5 you can use the `...` syntax to extend existing minimizers (i.e. `terser-webpack-plugin`), uncomment the next line
            // `...`,
            // css压缩
            new CssMinimizerPlugin(),
            /* 似乎使用了CSS压缩就会造成在生成模式下webpack无法压缩js代码, 于是使用这一内置插件用以压缩js代码 */
            /* 开发环境下请注释掉下面的代码 */
            // new TerserPlugin({
            //     parallel: true, // 可省略，默认开启并行
            //     terserOptions: {
            //         toplevel: true, // 最高级别，删除无用代码
            //         ie8: true,
            //         safari10: true,
            //     }
            // })
        ]
    },
    cache: true, // 可省略，默认最优配置：生产环境，不缓存 false。开发环境，缓存到内存，memory
    devtool: process.env.NODE_ENV === 'production' ? 'eval-cheap-module-source-map' : 'inline-source-map', // 生产环境和开发环境的配置
    plugins: [
        new MiniCssExtractPlugin({
            // 输出到dist/css目录,且文件名为10位hash值
            filename: './css/[name].css',
        }),
        new ESLintPlugin({
            context: resolve(__dirname), //指定文件根目录，类型为字符串
            extensions: 'js', // 指定需要检查的扩展名 ,默认js
            exclude: '/node_modules/', // 排除node_modules文件夹, 配置里可以只保留这一项
            fix: false, // 启用 ESLint 自动修复特性。 小心: 该选项会修改源文件。
            quiet: false, // 设置为 true 后，仅处理和报告错误，忽略警告
        }),
        new HtmlWebpackPlugin({
            template: './index.html' // 以index.html为模板
        })
    ],

    /* dev-serve 自动化编译 */
    devServer: {
        hot: true, // 热更新
        static: { //托管静态资源文件
            directory: resolve(__dirname, "/src"),
        },
        compress: true, //是否启动压缩 gzip
        port: 8080, // 端口号
        open: true,  // 是否自动打开浏览器
        client: { //在浏览器端打印编译进度
            progress: true,
        },
    },
}
```


## 准备生产环境
- 在项目根目录下创建config目录
- 在config目录下创建`webpack.dev.js`与`webpack.prod.js`两个文件
- 将`webpack.config.js`复制到两个文件中
- 修改`webpack.dev.js`中的mode,devtool,注释掉optimization中的js压缩; 修改`webpack.prod.js`中的mode,devtool,取消掉optiomization中的js压缩注释, 注释掉dev-server
- 修改package.json
```json
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "serve": "webpack serve",
    "dev": "webpack server --config ./config/webpack.dev.js",
    "build": "webpack --config ./config/webpack.prod.js"
  },
```
- 调用`npm run dev`与`npm run build`来调用这两种不同模式的配置
- 在两个文件的output添加publicPath并修改path, 生成的dist目录应该和config在同一级
  - 修改publicPath的原因是: 项目上线不要出现任何相对路径, `publicPath:'/'`代表的是`127.0.0.1:5000`根目录,
  - 打包后的css文件和js文件地址为`<link href="/css/8148d9e3.css" rel="stylesheet">`和`<script defer="defer" src="/js/bundle.js"></script>`,
  - css中的图片资源地址为`background-image:url(/media/img/e6f03d1d.jpg);`
```javascript
    output: { // 出口
        filename: 'js/bundle.js', // 输出文件名
        path: resolve(__dirname, '../dist'),// 输出文件路径配置
        clean: true, // 每次打包前都清空dist文件夹
        publicPath: "/"
    },
```

- 下载serve: `npm i serve -g`
- 执行生产打包, 生成dist打包文件夹
- 调用命令: `serve serve -s dist -p 5000`, 在`127.0.0.1:5000`中测试能否顺利打开文件, 资源引入是否成功




