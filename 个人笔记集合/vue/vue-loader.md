[Toc]

- >  vue@2.6.14使用vue-loader@14.2.4版本, 截止2022年3月20日,版本更新至@17.0.0

 # 用来干什么

- 通过 **vue-loader**， **webpack** 可以将 **.vue 文件** 转化为 **浏览器可识别的javascript**。

# 下载

- `npm i vue-loader@14.2.4 vue-template-compiler -D`

# 配置

## resolve配置

- ```js
  resolve:{
      extensions:['.js','.vue','.json'],
      alias:{'vue$':'vue/dist/vue.esm.js'}
  }
  ```

- 