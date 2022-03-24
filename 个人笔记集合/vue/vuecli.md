[Toc]

# 安装

- `npm i vue@2.5.16 -S`
- `npm i @vue/cli@4.5.15 -D`
- 本地安装后, 使用`npx vue -V`来查看是否安装成功



# 配置package.json

- 我觉的`npx vue create [project_name]`有些麻烦

- 所以在package.json中配置了

  - ```json
    	"scripts": {
    		"test": "echo \"Error: no test specified\" && exit 1",
    		"create": "npx vue create"
    	},
    ```

- 使用`npm run create vue_test1`来创建`vue_test1`项目



# 创建项目

- please pick a preset
  - 我选择 `default vue2`
  - 然后等待安装

- 修改命令: 我习惯用 npm run dev 来启动开发服务器, 默认是serve ,改为dev

  - ```json
    
    	"scripts": {
    		"dev": "vue-cli-service serve",
    		"build": "vue-cli-service build",
    		"lint": "vue-cli-service lint"
    	},
    ```



# 创建vue.config.js文件

- 在项目的根目录下创建`vue.config.js`

- ```js
  const {resolve} =require('path')
  module.exports = {
  	/* 写原生的webpack命令 */
  	configureWebpack: {
  		resolve: {
  			extensions: ['.js', '.vue', '.json'], // 可以省略的后缀名
  			alias: {
  				// 路径别名
  				vue$: 'vue/dist/vue.esm.js', // 表示精准匹配 带vue编辑器
  				'@': resolve(__dirname, '../src'), // 路径别名
  				'@components': resolve(__dirname, './src/components'),
  				'@pages': resolve(__dirname, '../src/assets/pages')
  			}
  		}
  		/* 还可以配置proxy代理,如果有必要的话 */
  	}
  }
  ```



# 配置babel.config.js

- 下载

  - `npm i babel-plugin-component -D`

- 配置

  - ```js
    module.exports = {
        presets: ['@vue/cli-plugin-babel/preset'],
        plugins: [
            [
                'babel-plugin-component',
                {
                    libraryName: 'mint-ui', // 针对mint-ui库实现按需打包
                    style: true, // 自动打包对应css
                },
            ],
        ],
    }
    ```

  - 