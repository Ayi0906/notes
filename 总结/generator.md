[Toc]

- 总结
  - yield 既是一个关键字 又是一个参数
  - 执行流读取到yield时便跳出, 返回一个`{value:  , done:  }`对象
    - 将yield后面紧跟的 表达式的值或值本身 赋值给对象, 并返回这个对象
    - 再次执行时,将`it.next()`接收的参数赋值给`yield` , 然后从yield 下一行代码开始执行,如果遇到yield, 再如上流程 

- 测试代码一:`yield`是一个参数

  - ```js
    function* gen(x) {
    	var y = x * (yield 77)
    	return y
    }
    
    let it = gen(5) // 构造迭代器
    console.log(it.next()) // {value:77,done:false}
    console.log(it.next(8)) // {value:40,done:true}
    ```

  - 默认丢弃第一个`next()`方法接收的参数

  - 如果没有返回任何值, 每一个函数都有一隐式地`return undefined`

- 测试代码二:每一个迭代器的数据都是独立的

  - ```js
    function* gen() {
    	var x = yield 2
    	z++
    	var y = yield x * z
    	console.log(x, y, z)
    }
    
    var z = 1
    let it1 = gen()
    let it2 = gen()
    let val1 = it1.next().value
    console.log(val1) // 2
    let val2 = it2.next().value
    console.log(val2) // 2
    val1 = it1.next(val2 * 10).value // {x:20,z:2}
    console.log(val1) // (20*2)=>40
    val2 = it2.next(val1 * 5).value // {x:200,z:3} // z是通用的
    console.log(val2) // (40*5*3)=>600
    it1.next(val2 / 2) // 20 300 3 // {x:20,y:300,z:3}
    it2.next(val1 / 4) // 200 10 3 // {x:200,y:10,z:3}
    ```
  
- 测试代码三:迭代器的复杂计算

  - ```js
    var a = 1
    var b = 2
    function* foo() {
    	a++
    	yield
    	b = b * a
    	a = (yield b) + 3
    }
    
    function* bar() {
    	b--
    	yield
    	a = (yield 8) + b
    	b = a * (yield 2)
    }
    
    function step(gen) {
    	var it = gen()
    	var last
    	return function () {
    		// 不管yield出来的是什么,下一次都把它原样传回去
    		last = it.next(last).value
    	}
    }
    
    var s1 = step(foo)
    var s2 = step(bar)
    
    function* baz() {
    	yield 5
    }
    // 首次运行*foo
    s1() // {value:undefined,done:false} // {a:2} {last:undefined}
    s1() // {value:4,done:false} // {last:4} // {b:2*2}
    s1() // {value:undefined,done:true} // {a:4+3}
    
    // 再运行*bar
    s2() // {value:undefined,done:false} // {b:3}
    s2() // {value:8,done:false} // {last:8}
    s2() // {value:2,done:false} // {last:2} // {a=8+3=11}
    s2() // {value:undefined,done:true} // {last:undefined}  // {b=a*2=22}
    console.log(a, b)
    ```

  - ```js
    var a = 1
    var b = 2
    function* foo() {
    	a++
    	yield
    	b = b * a
    	a = (yield b) + 3
    }
    
    function* bar() {
    	b--
    	yield
    	a = (yield 8) + b
    	b = a * (yield 2)
    }
    
    function step(gen) {
    	var it = gen()
    	var last
    	return function () {
    		// 不管yield出来的是什么,下一次都把它原样传回去
    		last = it.next(last).value
    	}
    }
    
    var s1 = step(foo)
    var s2 = step(bar)
    
    s2() // {value:undefined,done:false} // {b:1} // {last:undefined}
    s2() // {value:8,done:false} // {last:8}
    s2() // {value:2,done:false} // {last:2} // {a=8+1=9}
    s2() // {value:undefined,done:true} // {b=9*2=18}
    
    s1() // {value:undefined,done:false} // {last:undefined} // {a:10}
    s1() // {value:180,done:false} // {b:18*10=180} // {last:180}
    s1() // {value:undefined} // {a=180+3=183}
    console.log(a, b) // {183,180}
    ```

  - ```js
    function* gen() {
    	yield console.log('22222')
    	console.log('33333')
    	yield console.log('44444')
    }
    
    const it = gen()
    it.next() // 2222
    it.next() // 3333 4444
    ```
  
  - ```js
    function fn(num) {
    	console.log(num)
    	return num
    }
    
    function* gen() {
    	yield fn(1)
    	yield fn(2)
    	return 3
    }
    
    const it = gen()
    console.log(it.next())
    console.log(it.next())
    console.log(it.next())
    
    // 1
    // { value: 1, done: false }
    // 2
    // { value: 2, done: false }
    // { value: 3, done: true }
    ```
  
  - ```js
    function fn(num) {
    	return new Promise(resolve => {
    		setTimeout(() => {
    			resolve(num)
    		}, 1000)
    	})
    }
    
    function* gen() {
    	yield fn(1)
    	yield fn(2)
    	return 3
    }
    
    const it = gen()
    console.log(it.next())
    console.log(it.next())
    console.log(it.next())
    
    // { value: Promise { <pending> }, done: false }
    // { value: Promise { <pending> }, done: false }
    // { value: 3, done: true }
    
    ```
  
- 测试代码三

  - 使用generator函数改写async/await

    - ```js
      function fn(num) {
      	return new Promise(resolve => {
      		setTimeout(() => {
      			resolve(num * 2)
      		}, 1000)
      	})
      }
      
      async function fn2() {
      	const res1 = await fn(1)
      	const res2 = await fn(res1)
      	const res3 = await fn(res2)
      	return res3
      }
      
      fn2().then(res => {
      	console.log(res)
      })
      ```

    - ```js
      function fn(num) {
      	return new Promise(resolve => {
      		setTimeout(() => {
      			resolve(num * 2)
      		}, 1000)
      	})
      }
      
      fn2().then(res => {
      	console.log(res)
      })
      
      function* gen() {
      	const num1 = yield fn(1)
      	const num2 = yield fn(num1)
      	const num3 = yield fn(num2)
      	return num3
      }
      
      const it = gen()
      const next1 = it.next()
      next1.value.then(res1 => {
      	console.log(next1)
      	console.log(res1)
      	const next2 = it.next(res1)
      	next2.value.then(res2 => {
      		console.log(next2)
      		console.log(res2)
      		const next3 = it.next(res2)
      		next3.value.then(res3 => {
      			console.log(next3)
      			console.log(res3)
      			console.log(it.next(res3))
      		})
      	})
      })
      ```

    - 