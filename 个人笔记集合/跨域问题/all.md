[Toc]

# 跨域简述

- `http://127.0.0.1:3000`向`https://196.168.0.152:4000`发送请求, 因为协议, 统一资源定位符, 端口号是哪个只要有一个不一致就会产生跨域

# get请求的跨域

- get请求跨域问题只可能因为上述三种不同
- get请求的数据都放在URL后面, 不存在content-type的问题

## get请求的跨域解决

- cors解决

- ```js
  /* 解决跨域 */
  app.get('*', (req, res, next) => {
  	res.setHeader('Access-Control-Allow-Origin', 'http://localhost:3000')
  	next()
  })
  ```

# post请求的跨域

- post在上述三项不同的情况下,还可能因为pre-flight的问题造成跨域, 因为axios的post请求默认以`Content-Type: application/json`发送, 这种请求头会触发pre-flight请求

- 以axios发送post代码为例, preflight请求头与响应头分别是这样的

  - ```
    // General
    Request URL: http://127.0.0.1:3008/test3
    Request Method: OPTIONS
    Status Code: 200 OK
    Remote Address: 127.0.0.1:3008
    Referrer Policy: strict-origin-when-cross-origin
    ```

  - ```
    // Request Headers
    Access-Control-Request-Headers: content-type
    Access-Control-Request-Method: POST
    Origin: http://localhost:3000
    ...省略部分
    ```

  - ```
    // Response Headers
    // 如果没有在服务器设置, 那么这些响应头都不会存在, 因为这些是我在服务器中设置的, 但它们是必要的
    Access-Control-Allow-Headers: Content-Type
    Access-Control-Allow-Methods: PUT,HEAD,POST,DELETE,OPTIONS
    Access-Control-Allow-Origin: http://localhost:3000
    ```

  - preflight请求不同于直接发起options请求, preflight请求是由浏览器自动发起, 它不存在跨域问题(但是如果你用options发起跨域请求就会有跨域问题), 只要发起preflight请求就会成功获取到响应

## 解决preflight请求

- ```js
  const instance = axios.create({
  	baseURL: 'http://127.0.0.1:3008'
  })
  
  const result = await instance.post('/test3', {
  	data: {
  		name: 'zs',
  		age: 25
  	}
  })
  console.log(result)
  ```

- ```js
  // /* 解决跨域 */
  app.post('/test3', (req, res, next) => {
  	res.setHeader('Access-Control-Allow-Origin', 'http://localhost:3000')
  	next()
  })
  
  /* 解决pre-flight请求 */
  app.options('/test3', (req, res, next) => {
  	console.log('触发preflight请求')
  	res.setHeader('Access-Control-Allow-Origin', 'http://localhost:3000')
  	res.setHeader('Access-Control-Allow-Headers', 'Content-Type')
  	res.setHeader('Access-Control-Allow-Methods', 'PUT,HEAD,POST,DELETE,OPTIONS')
  	next()
  })
  ```

- 这样就能够解决axios发起post请求造成的pre-flight请求和跨域请求的问题

## 避免axios发起post请求时自动发起perflight请求

- 通过在请求拦截器中修改数据格式的方式来避免发起preflight请求

1. 引入qs插件

   1. ```js
      <script src="./node_modules/qs/dist/qs.js"></script>
      ```

2. 拦截器中处理数据

   1. 未处理前的数据格式

      1. ```js
         // config.data
         {
             name:'zs',
             age:25
         }
         ```

   2. 处理后的数据格式

      1. ```js
         // config.data
         name=zs&age=25
         ```

      2. 这个数据其实就是将上面的数据进行urlencode编码后的数据

   3. 拦截器代码

      1. ```js
         // 添加请求拦截器
         instance.interceptors.request.use(
         	function (config) {
         		// 在发送请求之前做些什么
         		const data = config.data
         		if (data instanceof Object) {
         			config.data = Qs.stringify(data)
         		}
         		console.log('config:', config.data)
         		return config
         	},
         	function (error) {
         		// 对请求错误做些什么
         		return Promise.reject(error)
         	}
         )
         ```

      2. 对数据进行处理后, 将不再以json格式发送, 而是以`content-type:x/www-form-urlencoded`, 

3. 服务器进行跨域处理

   1. 虽然不再发送preflight请求, 但是依然要进行请求的跨域处理

   2. ```js
      // 服务器代码
      
      // 对接收到的json格式数据进行处理
      app.use(express.json())
      // 对接收到的urlencode格式数据进行处理
      app.use(express.urlencoded({ extended: true }))
      
      app.post('/test3', (req, res) => {
      	res.setHeader('Access-Control-Allow-Origin', 'http://localhost:3000') // 处理跨域
      	console.log('访问成功')
      	console.log(req.body)
      	setTimeout(() => {
      		res.send('999')
      	}, 1000)
      })
      ```

   3. 

## preflight请求

### preflight请求是什么

- 一般来说, preflight请求就是`OPTIONS`请求, 
- 它会在浏览器认为`即将要执行的请求可能会对服务器造成不可预知的影响时`，由浏览器自动发出。通过预检请求，浏览器能够知道当前的服务器是否允许执行即将要进行的请求，只有获得了允许，浏览器才会真正执行接下来的请求。 通常`preflight`请求不需要用户自己去管理和干预，它的发出的响应都是由浏览器和服务器自动管理的。

### 为什么会有perflight请求

- `preflight`预检请求就是为`CORS`服务的，是`CORS`规范中的一部分, 通过限制跨域访问，可以极大的提高网页的安全性。
- 同时对于不支持`CORS`的旧服务器，通过`preflight`请求确认对`CORS`的支持情况，来决定下一步的访问是否要继续，以免对服务器的数据产生不可预知的影响。

### 触发perflight请求的时机

- 以下五项要求只要有一项不满足就会发送preflight请求
  - 请求方法限制:  只允许`get`,`post`,`head`方法
  - 请求头限制: 
    - 只允许包含以下九种`Accept`, `Accept-Language` , `Content-Language`, `Content-Type` , `DPR` , `DownLink` , `Save-Data`, `Viewport-Width` , `Width`
  - Content-Type 限制
    - Content-Type只能包含以下三种类型 `text/plain` `multipart/form-data` `application/x-www-form-urlencoded`
  - XMLHttpRequestUpload对象限制
    - `XMLHttpRequestUpload`对象没有注册任何事件监听器
  - ReadableStream对象限制
    - 请求中不能使用`ReadableStream`对象

### preflight请求中的请求头与响应头

- 它的请求通常长这个样子:

```
Access-Control-Request-Headers: x-requested-with
Access-Control-Request-Method: POST
Origin: http://test.preflight.qq.com
```

- 这里面主要关心`origin` `Access-Control-Request-Method` `Access-Control-Request-Headers`这三个字段，依次代表访问来源、真实请求的方法和真实请求的请求头。

- 响应通常长这个样子:

```
Access-Control-Allow-Headers: Content-Type, Content-Length, Authorization, Accept, X-Requested-With
Access-Control-Allow-Origin: http://test.preflight.qq.com
Access-Control-Allow-Methods: POST, GET, OPTIONS, DELETE
Access-Control-Max-Age: 86400
复制代码
```

相对应的，响应里我们需要关心的是`Access-Control-Allow-Origin` `Access-Control-Allow-Headers` `Access-Control-Allow-Methods` 这三个字段，依次代表当前请求支持的访问域、支持的自定义请求头、支持的请求方法，如果即将执行的请求的任意一项不在支持范围内，浏览器就会自动放弃执行真实请求。同时抛出`CORS`错误。最后一项`Access-Control-Max-Age`代表该预检请求的有效期，在有效期内浏览器不会再为同一请求执行预检操作。

### 正确支持preflight请求

对于服务端开发来说，如果自己的请求可能会遇到有`preflight`请求的情况，我们需要怎么配置来支持`preflight`请求呢？ 通常来说，我们需要关注的还是最关键的三个字段，`Access-Control-Allow-Origin` `Access-Control-Allow-Headers` `Access-Control-Allow-Methods`。

- Access-Control-Allow-Origin 这个一般用于对跨域请求的支持，对于绝大多数请求来说，访问来源是固定的，这个字段配置为支持的访问来源即可。对于通用的公共接口，比如`图片上传`这种，可以配置为`*`。不过这样做的安全性会大大降低，通常不建议使用。
- Access-Control-Allow-Headers 这个是用于对允许的自定义请求头的配置，通常对于一个固定的服务，支持的自定义请求头是固定的，我们在请求的时候配置在这里即可。
- Access-Control-Allow-Methods 这个是用于对允许的请求方法的配置，通常对于一个固定的服务，支持的请求方法也是固定的，我们在请求的时候配置在这里即可。这里也不建议配置为`*`，会大大降低服务的安全性。通常来说，在当前设计的方法之外，加上`OPTIONS` `HEAD`即可。

建议的配置(以koa2为例):

```
  ctx.set("Access-Control-Allow-Origin", 支持访问的网站域)
  ctx.set("Access-Control-Allow-Credentials", true);
  ctx.set("Access-Control-Max-Age", 86400000);
  ctx.set("Access-Control-Allow-Methods", "OPTIONS, HEAD, 当前请求的实际方法");
  ctx.set("Access-Control-Allow-Headers", "Content-Type, Content-Length, Authorization, Accept, X-Requested-With");
复制代码
```

同时，特别的如果你使用`router.post()`这种简写的方式去开发后端服务，还需要显式的配置对`OPTIONS`请求的返回码，来使浏览器正确的处理`OPTIONS`请求。 通常建议使用`200(OK)`或者`204(No Content)`返回码，当然实际上所有2开头的合法返回码，都会被浏览器认为`OPTIONS`认为请求执行成功。

```
if (ctx.request.method === "OPTIONS") {
    ctx.response.status = 204
  }
```

