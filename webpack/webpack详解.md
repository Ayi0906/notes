## entry
### string
- 单入口
	- 打包形成一个chunk，输出一个bundle文件
	- 此时 chunk 名称默认是main
- array
	- 所有入口文件最终只会形成一个chunk，输出出去只有一个bundle文件
	- 只有在HMR功能中让html热更新生效
```
['./src/index.js','./src/add.js']
```

- object
	- 有几个入口文件就能形成几个chunk，输出几个bundle文件
	- 此时chunk的名称是 key 键名
```
{
	index:['./src/index.js','./src/count.js'],
    add:'./src/add.js'
}
```

## output
### filename
- 文件名称（指定名称+目录）
`filename:'js/bundle.js'`

### path
- 输出文件目录
`path:resolve('dist')`

### publicPath
- 决定img，css等资源引入的路径

### chunkFilename
- 决定非入口chunk的名称
- 非入口，在index.js文件中通过`import('./add').then(({default:add})=>{clg(add(1,2);)})`;引入文件，会被打包成0.js
`chunkFilename:'js/[name]_chunk.js'`

### library

`library:'[name]' // 整个库向外暴露的变量名`
`libraryTarget:'window' // 变量名添加到哪个上 browser`
`libraryTarget:'global' // 变量名添加到哪个上 node`
`libraryTarget:'commonjs'`

## module
- 单个loader用'loader'
- 多个loader用'use'
- exclude：排除node_modules下的js文件
- include:resolve('src')：只检查当前文件下js文件
- enforce:'pre':优先执行
- enforce：'post':延后执行
- oneOf:[]:以下配置只会生效一个

```
module:{
	rules:[
    	// loader 配置
        {}
    ]
}
```

## resolve
- 解析模块的规则


```
resolve:{
	// 配置解析模块路径别名:优点简写路径，缺点路径没有提示
	alias:{
    	$css:resolve('src/css')
    },
    //配置省略文件路径的后缀名
    extensions:['.js','.json','.css'],
    (比如说 import('src/index') 会一个一个找，从index.js找，再找index.json，直到找到一个匹配的，所有的文件名称不要重复)
    
    // 告诉webpack解析模块去哪个目录找
    modules:[resolve('../../node_modules),'node_modules']
}

// index.js
import($css/index.js);
```

## devServer
```
// 运行代码的目录
contentBase:
// 监视 contentBase 目录下的所有文件，一旦文件变化就会reload
watchContentBase:true
// 忽略文件
watchOptions:{
	ignored:/node_modules/
}
// 启动gzip压缩
compress:true
// 端口号
port:3000
// 域名
host:'localhost',
// 自动打开浏览器
open:true
// 开启HMR功能
hot:true
// 不要显示启动服务器日志信息
clientLogLevel:'none'
// 除了一些基本启动信息以外，其他内容都不要显示
quiet:true,
// 如果出错了，不要全屏提示
overlay:false
```