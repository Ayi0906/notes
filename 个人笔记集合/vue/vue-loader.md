[Toc]

- >  vue@2.6.14使用vue-loader@14.2.4版本, 截止2022年3月20日,版本更新至@17.0.0

 # 用来干什么

- 通过 **vue-loader**， **webpack** 可以将 **.vue 文件** 转化为 **浏览器可识别的javascript**。

# 下载

- ```json
          "vue-loader": "^15.9.8",
          "vue-template-compiler": "^2.5.16",
  
  
          "vue": "^2.5.16"
  ```

- vue与vue-template-complier保持版本一致

- vue2最多能适配vue-loader@15.9.8

- `npm i vue-loader@15.9.8 vue-template-complier@2.5.16 -D`

# 配置

## resolve

```js
    resolve: {
        extensions: ['.js', '.vue', '.json'], // 可以省略的后缀名
        alias: {
            // 路径别名
            vue$: 'vue/dist/vue.esm.js', // 表示精准匹配 带vue编辑器
            '@': resolve(__dirname, '../src'), // 路径别名
            '@components': resolve(__dirname, '../src/assets/components'),
            '@pages': resolve(__dirname, '../src/assets/pages'),
        },
    },
```

## rules

```js
            /* 打包.vue文件, 这个规则要写在rules的最前面,否则会报 Make sure the rule matching .vue files include vue-loader in its use. 错误 */
            {
                test: /\.vue$/,
                loader: 'vue-loader',
                options: {
                    transformAssetUrls: {
                        video: ['src', 'poster'],
                        source: 'src',
                        img: 'src',
                        image: ['xlink:href', 'href'],
                        use: ['xlink:href', 'href'],
                    },
                    productionMode: true,
                },
            },
```

## 引入

```js
// webpack.config.js
const { VueLoaderPlugin } = require('vue-loader')
```

## 插件使用

```
plugins:[
        new VueLoaderPlugin(),
]
```

## 修改.css文件配置

```js
/* 将css的编译配置提取出来 */
const commonCssCompilerLoader = [
    process.env.NODE_ENV !== 'production'
        ? 'vue-style-loader'
        : MiniCssExtractPlugin.loader,
    {
        loader: 'css-loader',
        options: {
            // importLoader配置借鉴于官网
            importLoaders: 1,
        },
    },
    'postcss-loader', // css兼容性处理
]
```

