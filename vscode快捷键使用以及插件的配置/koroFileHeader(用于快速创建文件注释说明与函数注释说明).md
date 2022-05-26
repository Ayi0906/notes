[Toc]

## 用途

- 用于快速创建函数注释与文件注释

## 用法

### 下载

- 商店搜索"koroFileHeader"下载

### 配置

- 进入设置,搜索"FileHeader.configObj",点击setting:json

- 增加如下配置

- ```
      "fileheader.customMade": { // 头部注释
          "Author": "huangl",
          "Date": "Do not edit",
          "LastEditors": "huangl",
          "LastEditTime": "Do not edit",
          "Description": "file content",
          "FilePath": "Do not edit" // 增加此项配置即可
      },
      "fileheader.cursorMode": { // 函数注释
          "description": "",
          "param": "params",
          "return": ""
      },
      "fileheader.configObj": {
          "autoAdd": true, // 默认开启自动添加头部注释，当文件没有设置头部注释时保存会自动添加
          "autoAlready": true, // 默认开启
          "prohibitAutoAdd": [
              "json",
              "md"
          ], // 禁止.json .md文件，自动添加头部注释
          "wideSame": false, // 设置为true开启
          "wideNum": 13 // 字段长度 默认为13
      }
  ```

  - 似乎要在cursorMode与customMode中配置而不是obj中配置

## 使用

### 文件注释

- 创建文件会自动添加头部注释, 如需要手动添加则`ctrl+win+i`

### 函数注释

- 在函数的上一行点击插入光标,使用`ctrl+win+t`创建函数注释

## 链接

[配置说明](https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE)