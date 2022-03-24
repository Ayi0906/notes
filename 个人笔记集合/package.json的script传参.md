# 使用npm_config环境变量（推荐）

- 

  ```json
  "scripts": {
   "view": "echo %npm_config_host% & echo %npm_config_port%",
  }
  ```

  - 执行`npm run view --host=localhost --port=2333`即可

# 调整命令格式

- 仅适用于变量少、并且可以调整命令格式的情况。 原理：将参数调整至末尾 例如想要执行`frida -U appname -l _agent.js`，其中`appname`是可变的，则调整命令格式为：

- ```json
  "scripts": {
      "attach": "frida -l _agent.js -U"
  }
  ```

  - 此时执行`npm run attach appname`即可

# js或ts

- ```json
  "scripts": {
      "test": "node test.js"
  }
  ```

- 执行`npm run test appname`，直接从`process.argv`获取即可：

  - ```js
    const args = process.argv.slice(2);
    console.log('process.argv：\n', process.argv)
    console.log('参数：\n', args)
    ```

  - 