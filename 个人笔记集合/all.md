[Toc]

# 1 计算机知识

## 1.1 比特位(bit),字节(byte)以及换算关系

### 1.1.1 基本知识

- bit是计算机内存中的最小单位, 每个bit可以代表0或1的数位讯号
  - 1bit只能储存1或0
  - 计算机系统中所有东西都是由0或1比特位构成
  - 1bit有些太小以至于无法使用, 因此将8bit组合在一起组成1Byte(字节), 可以存储2^8=256种数据, 代表[0,255]的整数数字

- Byte是计算机系统中最小的存储单位. Byte是计算机记忆体储存资料的基本单位, 当记忆体容量过大时，Byte 这个单位就不够用，因此就有KB\MB\GB等出现
- **1个英文字母**(不分大小写)或者 **1个阿拉伯数字(0~9)**通常占1个字节；
  - 1byte，如 01001000 表示英文字母 H 。
- **1个中文汉字**通常占2个字节；
- **标点符号：英文标点符号**占1个字节，**中文标点符号**占2个字节。

### 1.1.2 换算关系

- 1bit = 0或1
- 1Byte = 8bit // 保存256种数据
- 1KB = 1024Byte = 2^10 Byte 
- 1MB = 2^10KB = 1024 KB
- 1GB = 2^10MB = 1024MB

### 1.1.3 人类可读字符串转换成二进制编码示例

```
字符串：
Hello

二进制编码：
    H        e       l         l       o
01001000 01100101 01101100 01101100 01101111 
```

## 1.2 字符

### 1.2.1 Ascii

- > ASCII ((American Standard Code for Information Interchange): 美国信息交换标准代码）是基于拉丁字母的一套电脑编码系统，主要用于显示现代英语和其他西欧语言。它是最通用的信息交换标准，并等同于国际标准ISO/IEC 646。ASCII第一次以规范标准的类型发表是在1967年，最后一次更新则是在1986年，到目前为止共定义了128个字符
  >
  > 它最初是美国国家标准，供不同计算机在相互通信时用作共同遵守的西文字符编码标准，后来它被国际标准化组织（International Organization for Standardization, ISO）定为国际标准，称为ISO 646标准。适用于所有拉丁文字字母 。

- ASCII 码使用指定的7 位或8 位二进制数组合来表示128 或256 种可能的字符。标准ASCII 码也叫基础ASCII码，使用7 位二进制数（剩下的1位二进制为0）来表示所有的大写和小写字母，数字0 到9、标点符号，以及在美式英语中使用的特殊控制字符

- 部分Ascii字符表

| 二进制    | 八进制 | 十进制 | 十六进制 | 字符              | 解释      |
| --------- | ------ | ------ | -------- | ----------------- | --------- |
| 0000 0110 | 06     | 6      | 0x06     | ACK (acknowledge) | 收到通知  |
| 00010110  | 026    | 22     | 0x16     | SYN               | 同步空闲  |
| 0010 0000 | 040    | 32     | 0x20     | space             | 空格      |
| 0011 0000 | 060    | 48     | 0x30     | 0                 | 字符0     |
| 0100 0001 | 0101   | 65     | 0x41     | A                 | 大写字母A |
| 0110 0001 | 0141   | 97     | 0x61     | a                 | 小写字母a |
| 0111 1111 | 0177   | 127    | 0x7F     | DEL               | 删除      |



## 1.3 powershell与cmd的区别

Powershell是cmd的超集, cmd能做的事情，Powershell都能做，但是Powershell还能额外做许多cmd不能做的活。



# 2 html



 



# 3 javascript

## js中的进制

### js中表达八进制, 十六进制

- 对于16进制数据'25', 需要表达为 0x25; 对于8进制数据'25', 需要表达为 025

  - ```js
    console.log((0x25).toString(10)) // '37' 5*16^0+2*16^1=37
    console.log((025).toString(10)) // '21' 5*8^0+2*8^1=21
    ```

## Number类型



### 非数值转换为数值

#### Number

- 转型函数, 可用于任何数据类型

  - ```js
    console.log(Number(true)) // 1
    console.log(Number(null)) // 0
    console.log(Number(undefined)) // NaN
    console.log(Number('-5')) // -5
    console.log(Number('0000111')) // 111
    console.log(Number('+5b77')) // NaN
    console.log(Number('0xaa')) // 170
    console.log(Number('')) // 0
    console.log(Number('b5a')) //NaN
    console.log(
    	Number({
    		val: true,
    		valueOf() {
    			return this.val
    		}
    	})
    ) // 1
    ```

  - 对于对象来说, Number转型函数会调用这个对象的valueOf()方法, 并将返回的值进行转型

  - 转型规则总结:

    - | 类型      | 转换的值                                |
      | --------- | --------------------------------------- |
      | boolean   | true转1, false转0                       |
      | string    | 仅有数字/正负值/进制头/空字符串可以转换 |
      |           | 数字字符串前面的多个0会被省略           |
      | null      | 0                                       |
      | undefined | NaN                                     |
      | object    | 调用valueOf方法, 然后将返回值进行转换   |

#### parseInt

- ```js
  console.log(parseInt('1234aa')) // 1234
  console.log(parseInt('')) // NaN
  console.log(parseInt('0xaa')) // 170
  console.log(parseInt(22.5)) // 22
  console.log(parseInt('070')) // 70
  console.log(parseInt(070)) // 56
  console.log(parseInt('22.5')) // 22
  ```

- 规则总结

  - | 传入参数                           | 转换值             |
    | ---------------------------------- | ------------------ |
    | 任何第一个字符不是数值/加号/减号   | NaN                |
    | 以数字开头的, 带有其它字符的字符串 | 保留整数数字       |
    | 0x开头的数字或者字符串             | 十六进制数转十进制 |
    | 0开头的数字                        | 八进制数转十进制   |
    | 小数                               | 保留整数数字       |

- 接收第二个参数作为进制数, 所有的传入数都会被转化成十进制整数

  - ```js
    console.log(parseInt('88', 16)) // 136
    console.log(parseInt(55, 8)) // 45
    console.log(parseInt(1101, 2)) // 13
    ```

#### parseFloat

- 它始终忽略掉字符串开头的0

- ```js
  console.log(parseFloat('1234blue')) // 1234
  console.log(parseFloat('xA')) // NaN
  console.log(parseFloat('0xA')) // 0
  console.log(parseFloat('11.11')) // 11.11
  console.log(parseFloat('11.11.14')) // 11.11
  console.log(parseFloat('098.6')) // 98.6
  console.log(parseFloat('0.98')) // 0.98
  console.log(parseFloat('3.125e4')) // 31250
  ```

- 

## String类型

### 非字符串转换为字符串

两种方法: 直接调用toString() / 使用String()转型函数

- Number, Boolean, Object, String都有toString()方法, 可以使用它来将以上类型的值转换为字符串(null和undefined没有toString()方法)

  - 只有Number类型调用toString()时会接收参数表示要转换的进制, 默认转换为10进制

    - ```js
      let num = 10
      console.log(num.toString(2)) // '1010'
      console.log(num.toString(8)) // '12'
      console.log(num.toString(16)) // 'a'
      ```

  - 其它三种调用toString()方法的结果

    - ```js
      console.log({}.toString()) // '[object object]'
      console.log(true.toString()) // 'true'
      console.log(Symbol.toString()) // 'function Symbol() { [native code] }'
      ```

- String转型函数遵守以下规则

  - 如果该类型数据有toString()方法, 则调用toSting()方法

  - 如果是null或undefined类型, 则返回'null'或'undefined'

  - ```js
    console.log(String(null)) // 'null'
    console.log(String(undefined)) // 'undefined'
    ```



### 字符串转其它类型

#### 字符串转数组

- split(), String.prototype.split

- ```js
  let str = 'hello world'
  // 1.接收普通的字符
  console.log(str.split('o')) // [ 'hell', ' w', 'rld' ]
  
  // 2.接收普通字符并限制得到的数组的长度
  console.log(str.split('l', 1)) // [ 'he' ]
  
  // 3.接收一个正则表达式, 并限制得到的数组长度
  console.log(str.split(/\B/, 4)) // [ 'h', 'e', 'l', 'l' ]
  ```

## 7 集合引用类型

### 7.1 Object

- 对象字面量

  - ```js
    let obj={
        name:'Nick',
        age:15,
        18:true
    }
    ```

- 在对象字面量表示法中, 可以使用String或者Number类型数据作为key值. 但在实际保存中, Number类型的key值会被转换为String类型的key值

- 使用中括号来访问对象属性

  - ```js
    console.log(person["name"])
    // 或者
    const propertyName='name'
    console.log(person[propertyName])
    ```

### 7.2 Array

#### 7.2.1 创建数组

- 使用Array构造函数创建

  - ```js
    let colors=new Array(20) // length为20的空数组
    let colors=new Array('red','blue','orange') // length为3的数组, 包含传入的三个参数作为数组元素
    ```

  - 只传入一个值时根据传入值的类型进行判断, 数值创建空数组, 其它类型值作为数组元素进行保存

- 在使用Array构造函数时, 可以省略new操作符

- 数组字面量

  - ```js
    let colors=['red','black']
    ```

- es6新增的`Array.from()`将`类数组对象`和`可遍历对象`转换为数组
- es6新增的`Array.of()`将传入参数转换为数组, 并且完全可以使用`Array.of()`代替Array构造函数

### 属性的类型

#### 数据属性

#### 访问器属性

#### 遍历对象的属性

- Object.keys(Object)

  - 它只返回对象的实例属性, 且该属性必须是可迭代的{enumerable:true}

  - ```js
    function Person() {}
    Person.prototype.hair = 'blue'
    let person1 = new Person()
    person1.name = 'Nick'
    Object.defineProperty(person1, 'age', {
    	value: 25,
    	writable: true,
    	enumerable: false,
    	configurable: true
    })
    
    console.log(Object.keys(person1)) // ['name']
    ```

  - 

- Object.getOwnPropertyNames(Object)

  - 它可以返回对象的所有实例属性, 无论是否可以枚举, 比如Object.prototype对象的实例属性全部不可迭代

  - ```js
    console.log(Object.getOwnPropertyNames(Object.prototype))
    ```

  - ```js
    [
      'constructor',
      '__defineGetter__',
      '__defineSetter__',
      'hasOwnProperty',
      '__lookupGetter__',
      '__lookupSetter__',
      'isPrototypeOf',
      'propertyIsEnumerable',
      'toString',
      'valueOf',
      '__proto__',
      'toLocaleString'
    ]
    ```

### Array

#### 数组转字符串

- join方法, Array.prototype.join

  - ```js
    let arr = [3, 5, 7, 9]
    console.log(arr.join('/')) // 3/5/7/9
    ```

##### 栈方法_在数组末尾进行增删项

- push方法

  - ```js
    let arr = [3, 5]
    console.log(arr.push(6, 7, 8)) // 5
    console.log(arr) // [ 3, 5, 6, 7, 8 ]
    ```

  - 在数组尾部添加任意多的项, 返回新数组的length

- pop方法

  - ```js
    let arr = [3, 5, 7]
    console.log(arr.pop()) // 7
    console.log(arr) // [ 3, 5 ]
    ```

  - 在数组尾端删除最后一项, 不接受参数, 返回被删除的项

# es6

## 8 数组的扩展

### 8.2 Array.from

- 用于将`类数组对象`和`可遍历对象`转换为数组实例

  - 将`类数组对象`转换为数组

    - ```js
      let arrayLike={
          0:'red',
          1:'green',
          2:'blue',
          length:3
      }
      // ES5的转换写法
      [].slice.call(arrayLike)
      
      // ES6写法
      Array.from(arrayLikeObj)
      ```

    - 实际应用中常见类数组对象是DOM操作返回的NodeList集合, 以及函数内部的arguments对象

      - ```js
        // NodeList集合转换数组
        let aP=document.querySelectAll('p')
        Array.from(ap).forEach(item=>{
            
        })
        ```

      - ```js
        // arguments对象转换数组
        function foo(arg1,arg2,arg3){
            let argsArr=Array.from(arguments)
        }
        ```

      - 扩展运算符也可以实现这一功能

        - ```js
          // NodeList集合转换数组
          let aP=[...document.querySelectAll('p')]
          
          // arguments对象转换数组
          function foo(arg1,arg2,arg3){
              let argsArr=[...arguments]
          }
          ```

        - `Array.from` 与扩展运算符的区别

          - 任何有length属性的对象都可以使用Array.from进行数组转换, 但是扩展运算符不行

  - 将可遍历对象转换为数组

    - 只要是部署了Iterator接口的数据结构都可以转为数组

      - ```js
        Array.from('hello') // ['h','e','l','l','o']
        ```

- 接收第二个参数, 类似于数组的map方法, 对每一个元素进行处理, 然后将新元素放入到数组中

  - ```js
    const a1 = [1, 2, 3, 4]
    const a2 = Array.from(a1, x => x * 2)
    console.log(a2) // [ 2, 4, 6, 8 ]
    
    // 第三个参数, 用于指定函数中this的值, 但这个重写的this在箭头函数中不适用
    const a3 = Array.from(
    	a1,
    	function (x) {
    		return x ** this.exponent
    	},
    	{ exponent: 2 }
    )
    
    console.log(a3) // [ 1, 4, 9, 16 ]
    ```

- Array.from() 对传入数组执行深复制

  - ```js
    let arr = [1, 5, 36]
    let arr1 = Array.from(arr)
    console.log(arr1 === arr) // false
    ```

### 8.3 Array.of

- 用于将一组参数转换为数组实例

  - ```js
    let arr = Array.of(3, 5, 7)
    console.log(arr) // [ 3, 5, 7 ]
    ```

  - 这个方法用于弥补数组构造函数Array()因为参数的个数不同导致的行为差异; 它可以用来替代Array构造函数; `Array.of()`总是返回参数值组成的数组, 如果没有参数, 就返回一个空数组

## import与export

- 分别暴露

  - ```js
    // 分别暴露
    export const a = { num1: 77, num: 22 }
    export const b = { num1: 55, num2: 66 }
    export function fn() {
    	console.log('分别暴露')
    }
    ```

  - 使用分别引入

    - ```js
      // 分别引入
      import { fn, a, b } from './inport1'
      console.log(fn, a, b) // [Function: fn] { num1: 77, num: 22 } { num1: 55, num2: 66 }
      ```

    - 暴露顺序不影响引入顺序, 但是暴露的对象与引入对象必须同名

  - 使用统一引入

    - ```js
      // 统一引入
      import * as obj from './inport1'
      console.log(obj)
      /* 
      {
        fn: [Function: fn],
        a: { num1: 77, num: 22 },
        b: { num1: 55, num2: 66 }
      }
      */
      ```

- 统一暴露

  - ```js
    // 统一暴露
    const a = { num1: 77, num: 22 }
    const b = { num1: 55, num2: 66 }
    function fn() {
    	console.log('分别暴露')
    }
    export { a, b, fn }
    ```

  - 使用分别引入与统一引入, 与分别暴露一致

- 默认暴露

  - 默认暴露的变量名可有可无

    - ```js
      // 默认暴露
      export default function () {
      	console.log('默认暴露')
      }
      ```

    - ```js
      import fn from './inport1'
      fn() // 默认暴露
      ```

    - 引入的时候变量名可以随便写

- 混合暴露

  - ```js
    // 分别暴露
    export function foo() {}
    export function bar() {}
    
    // 统一暴露
    function demo() {}
    let obj = {}
    export { demo, obj }
    
    // 默认暴露
    export default { a: 7, b: 8 }
    ```

  - 使用分别引入

    - ```js
      import defaultModule, { foo, bar, demo, obj } from './inport1'
      console.log(defaultModule, foo, bar, demo, obj) // { a: 7, b: 8 } [Function: foo] [Function: bar] [Function: demo] {}
      ```

    - 使用分别引入的时候要注意默认暴露必须写在最前面

  - 使用统一引入

    - ```js
      import * as moduleObj from './inport1'
      console.log(Object.keys(moduleObj)) // [ 'foo', 'bar', 'demo', 'obj', 'default' ]
      ```

    - 使用统一引入使用 moduleObj.default 来获取

- 快速判断是什么引入与暴露方法

  - `import { [propertyName] } from [地址]` // 带括号的一定是分别引入
  - `import module from [地址]` // 不带括号的一定是默认引入
  - `import * as module from [地址]` // 统一引入, 三种暴露方法都可以引入, 无法判断

## padStart与padEnd

- ```js
  let str = '55'
  console.log(str.padStart(5, '0')) // 00055
  console.log(str.padEnd(6, '1')) // 551111
  ```

# promise

## promise基础

1. `Promise()`构造函数通过new 操作符来实例化, 且必须传入一个执行器(executor)函数作为参数, 如果不传入执行器函数会抛出错误( 所以哪怕传入一个空函数也不可以不传入 )

```js
let p=new Prmoise(()=>{})
setTimeout(console.log,0,p) // Promise <pending>
```

2. promise有三种状态: 待定(pending), 兑现(resolved), 拒绝(rejected)
3. 执行器函数是同步执行的
4. promise状态切换为兑现, 就会有一个私有的内部值(value); 只要状态切换为拒绝, 

## 打断promise链条





# atob与btoa函数

- atob() : 解码一个Base64字符串
- btoa() : 从一个字符串或者二进制数据编码一个Base64字符串



# css

## opacity:0 与display: none 与 visiblity:hidden 的区别

- 相同作用: 都是对元素进行隐藏
- 不同处:
  - 空间占据: 
    - {display: none}  不占据空间, 动态改变此属性时会引起重排
    - {visibility: hidden} 元素会被隐藏, 但是不会消失, 依然占据空间
    - {opacity:0} 透明度为100%, 元素隐藏, 依然占据空间
  - 继承性:
    - {display:none} 不会被子元素继承, 但是父元素不显示, 子元素也不会显示
    - {visibility: hidden} 会被子元素继承, 可以通过设置子元素 {visibility: visible} 使子元素显示出来
    - {opacity:0} 也会被子元素继承, 但是不能通过设置子元素 {opacity:1} 使子元素重新显示出来
  - 动画效果:
    - display 与 opacity 可以实现动画效果
    - visiblity比较特殊:
      - 设置了{visibility : hidden} 的元素无法通过类似 :hover{visibility: visible} 的方法去实现复现. 通过js操作DOM修改visibility的属性值也没有transition-duration的任何效果( 是立刻呈现的效果 )
      - 设置了{visibility : visible} 的元素可以通过 :hover{visibility: hidden} 的方法去实现隐藏, 但是transition-duration属性的设置呈现 transition-delay 的效果, 也就是说在start到end的时间里, 只有第一帧到倒数第二帧都保持显示, 最后一帧被隐藏.



# nodejs

# express

### res

- res.json

- res.status

  - 设置状态码

  - ```js
    res.status(401)
    ```

  - 


## buffer缓冲区

### 什么是buffer

- > 在引入 TypedArray 之前，JavaScript 语言没有用于读取或操作二进制数据流的机制。 Buffer 类是作为 Node.js API 的一部分引入的，用于在 TCP 流、文件系统操作、以及其他上下文中与八位字节流进行交互。

- 总结起来一句话 **Node.js 可以用来处理二进制流数据或者与之进行交互**。

#### 什么是二进制数据？

- 8421码

#### 什么是 Stream？

- 流，英文 Stream 是对输入输出设备的抽象，这里的设备可以是文件、网络、内存等。
- 流是有方向性的，当程序从某个数据源读入数据，会开启一个输入流，这里的数据源可以是文件或者网络等，例如我们从 a.txt 文件读入数据。相反的当我们的程序需要写出数据到指定数据源（文件、网络等）时，则开启一个输出流。当有一些大文件操作时，我们就需要 Stream 像管道一样，一点一点的将数据流出。
- 我们现在有一大罐水需要浇一片菜地，如果我们将水罐的水一下全部倒入菜地，首先得需要有多么大的力气（这里的力气好比计算机中的硬件性能）才可搬得动。如果，我们拿来了水管将水一点一点流入我们的菜地，这个时候不要这么大力气就可完成。

#### 再次说明: 什么是buffer

- 通过以上 Stream 的讲解，我们已经看到数据是从一端流向另一端，那么他们是如何流动的呢？

  通常，数据的移动是为了处理或者读取它，并根据它进行决策。伴随着时间的推移，每一个过程都会有一个最小或最大数据量。如果数据到达的速度比进程消耗的速度快，那么少数早到达的数据会处于等待区等候被处理。反之，如果数据到达的速度比进程消耗的数据慢，那么早先到达的数据需要等待一定量的数据到达之后才能被处理。

  这里的等待区就指的缓冲区（Buffer），它是计算机中的一个小物理单位，通常位于计算机的 RAM 中。这些概念可能会很难理解，不要担心下面通过一个例子进一步说明。

#### 公共汽车站乘车例子

- 举一个公共汽车站乘车的例子，通常公共汽车会每隔几十分钟一趟，在这个时间到达之前就算乘客已经满了，车辆也不会提前发车，早到的乘客就需要先在车站进行等待。假设到达的乘客过多，后到的一部分则需要在公共汽车站等待下一趟车驶来。
- 在上面例子中的等待区公共汽车站，对应到我们的 Node.js 中也就是缓冲区（Buffer），另外乘客到达的速度是我们不能控制的，我们能控制的也只有何时发车，对应到我们的程序中就是我们无法控制数据流到达的时间，可以做的是能决定何时发送数据。

### buffer的基本使用

- 通过 Buffer.from()、Buffer.alloc() 与 Buffer.allocUnsafe() 三种方式来创建

- 根据老师的说法, 当对象赋值为null, 对象就成了待清理数据, js清理垃圾是按时清理的. 当堆内存中的某一块内存被标记为垃圾时, 此时调用new Buffer() (**注意它已被废弃**) 创建堆内存数据, 堆内存随机开辟一块内存供其使用储存, 可能就开到这块被标记为垃圾的堆内存处. new Buffer不论是否会发生这种情况, 都会对开辟的空间做清扫之后才储存数据, 因此它性能差. Buffer.alloc()强行要求堆内存开辟干净的未被占用的内存.Buffer.allocUnsafe()则不论是否有数据, 也不执行清理强行写入, 可能造成数据混乱. 打印Buffer.allocUnasfe(10), 可以看到每次打印都不一样
- 输出的buffer不是二进制, 输出是16进制数据, 储存用2进制数据, 输出自动转16进制

#### 将字符串进行二进制转换

- ```js
  let buf1 = Buffer.from('hello world')
  console.log(buf1) // <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64>
  ```

#### 创建一个不包含数据的指定存储大小的缓冲区

- ```
  let buf2 = Buffer.alloc(10)
  console.log(buf2) // <Buffer 00 00 00 00 00 00 00 00 00 00>
  buf2.fill(50)
  console.log(buf2) // <Buffer 32 32 32 32 32 32 32 32 32 32>
  ```

- 上面创建的是一个大小为10字节的缓冲区

- 可以通过Buffer.fill()使用某个值进行填充

#### nodejs中汉字默认使用utf-8编码, 因此每个汉字占3个字节

- ```
  let buf3 = Buffer.from('哈哈')
  console.log(buf3) // <Buffer e5 93 88 e5 93 88>
  ```

#### 可以通过buf[index]来获取buffer中指定的某个数据

- ```js
  let buf3 = Buffer.from('哈哈')
  console.log(buf3) // <Buffer e5 93 88 e5 93 88>
  console.log(buf3[1]) // 147 9*16^1+3*16^0=147
  ```

#### buffer.toString([encoding[,start[,end]]])转化为指定编码的字符串

- encoding: 使用的字符编码, 默认utf8

- start: 要解码的字节段的首位

- end: 要解码的字节段的末位

- 解码按照 `[start,end)` 左闭右开进行取样

- ```js
  let buf4 = Buffer.from('明月松间照')
  console.log(buf4.toString('utf8', 0, 6)) // 明月
  ```

#### Buffer.slice([start,[end.[type]]]) 截取buffer

- 左闭右开

- type: string , 新blob的内容类型

- 创建并返回包含此 `Blob` 对象数据子集的新 `Blob`。 原 `Blob` 没有改动。

- ```js
  let buf5 = Buffer.from([1, 2, 3, 4, 5])
  let sliceBuffer = buf5.slice(0, 1) // 浅复制
  sliceBuffer[0] = 100
  console.log(buf5) // <Buffer 64 02 03 04 05>
  ```



#### Buffer.copy(`target[, targetStart[, sourceStart[, sourceEnd]]]`)

- `target`  要复制到的 `Buffer` 或 [`Uint8Array`](http://url.nodejs.cn/ZbDkpm)。

- `targetStart` `target` 内开始写入的偏移量。 **默认值:** `0`。

- `sourceStart`  `buf` 内开始复制的偏移量。 **默认值:** `0`。 左闭右开

- `sourceEnd`  `buf` 内停止复制的偏移量（不包括）。 **默认值:** [`buf.length`](http://nodejs.cn/api-v16/buffer.html#buflength).

- 返回:  复制的字节数。

- ```js
  let buf1 = Buffer.from('明月松间照')
  let buf2 = Buffer.from('举杯邀某某,对影成三人')
  // 将buf1 字节[0,6)复制到buf2中,以buf2的9字节开始替换
  buf1.copy(buf2, 9, 0, 6)
  console.log(buf2.toString()) // 举杯邀明月,对影成三人
  ```

#### Buffer.concat拼接

- ```js
  let buf1 = Buffer.from('明月')
  let buf2 = Buffer.from('出天山')
  let buf3 = Buffer.concat([buf1, buf2])
  console.log(buf3.toString()) // 明月出天山
  let buf4 = Buffer.from('松间照')
  let buf5 = Buffer.concat([buf1, buf4], 12) // 限定12个字节长度
  console.log(buf5.toString()) // 明月松间
  ```

#### Buffer.isBuffer

- ```js
  let buf1 = Buffer.alloc(10)
  console.log(Buffer.isBuffer(buf1)) // true
  ```

### Buffer的特点/性质

- Buffeer的效率很高, 存储和读取很快, 因为它直接对计算机内存进行操作

- Buffer的大小一经确定, 不可修改

- Buffer的每个元素占用内存大小为1字节(byte)

  - 因为读取的每个元素都是16进制, 每个元素由两个16进制数据表示, 可以表达的数据是16*16=256种

- Buffer是node的核心模块, 不需要引入就可以在全局作用域中使用, 但仍然建议通过import或require引入

  - ```js
    import { Buffer } from 'buffer';
    ```

## base64编解码

### 编码原理

- 先定义一个编码字符数组(这里使用字符串)

  - ```js
    let str = 'ABCDEFGHIJKLMNOPKRSTUVWXYZ'
    str += str.toLocaleLowerCase()
    str += '0123456789+/'
    ```

- 将待转换的数据每三个字节(byte)分为一组, 每个字节占8bit, 共有24个二进制位

- 将24个二进制位每六个一组, 分为4个组

- 在每一组前面加上两个0, 每组由6个变为8个二进制位, 总共32个二进制位, 即4个字节

- 这一组每个字节都进行十进制转换, 从编码字符数组中找到对应的字符进行组合

- 对于最后的部分无法凑齐24个二进制位的部分, 比如只剩下15位, 前12位为2组, 后面3位为第三组, 假设为 001, 那么补足为00100000 , 对于第四组没有任何数据, 则以=表示

###  手写编码函数

- ```js
  function encodeBase64(strParam) {
  	if (!typeof strParam === 'String') {
  		return console.log('仅接受字符串参数')
  	}
  	let buf1 = Buffer.from(strParam)
  	let str = 'ABCDEFGHIJKLMNOPKRSTUVWXYZ'
  	str += str.toLocaleLowerCase()
  	str += '0123456789+/'
  	let ascii = ''
  	let base64 = ''
  	// 将数据按照每三个字节分为一组,每个字节占8个二进制位, 一组共有24个二进制位, 即
  	for (let i = 0; i < buf1.length; i++) {
  		ascii += buf1[i].toString(2).padStart(8, '0') // 将所有的16进制数值转换为2进制数值的字符串
  	}
  	for (let i = 0; i < ascii.length; i += 24) {
  		// 每24位二进制位为一组
  		let curBinArr = ascii.slice(i, i + 24) // [0,24) [24,48)
  		let lastBin = ''
  		// 将24位分成4组,每组6位
  		for (let j = 0; j < curBinArr.length; j += 6) {
  			let curBin = curBinArr.slice(j, j + 6)
  			if (curBin.length !== 6) {
  				lastBin = curBin
  				break
  			} else {
  				curBin = curBin.padStart(8, '0')
  			}
  			let index = parseInt(curBin, 2)
  			base64 += str[index]
  		}
  
  		if (curBinArr.length !== 24) {
  			// 最后的数据, 不一定有四组, 如果没有四组, 剩下的以=号补充
  			// 计算有多少个等号?
  			let index = parseInt(('00' + lastBin).padEnd(8, '0'), 2)
  			base64 += str[index]
  			// 计算当前有多少组
  			let num = 4 - Math.ceil(curBinArr.length / 6)
  			for (let j = 0; j < num; j++) {
  				base64 += '='
  			}
  		}
  	}
  	return base64
  }
  
  console.log(encodeBase64('https://baidu.com'))
  ```

- js已经有了转换API, 详见 `atob`, `btoa`





# mongodb





## mongoose插件

### 下载

- `npm i mongoose`

### 连接指定数据库

- ```js
  const mongoose= require('mongoose')
  const db=mongoose.connect('mongodb://localhost:27017/test',{
  	keepAlive:120 // 对于长期运行的后台应用，启用毫秒级 keepAlive 是一个精明的操作。不这么做你可能会经常 收到看似毫无原因的 "connection closed" 错误
  })
  ```
  
- `connect()`返回一个状态待定的(pending)的连接

### 监听连接的状态

- ```js
  db.on('error', console.error.bind(console, 'connection error:'));
  db.once('open', function() {
    // we're connected!
  });
  ```

### 创建架构(Schema)

- ```js
  let Schema=mongoose.Schema // 引入架构对象
  let myRule=new Schema({
      stu_id:{
          type:String, // 限制字段值必须为字符串
          required:true, // 限制字段是必须的
          unique:true // 限制字段值是唯一的
      },
      age:{
          type:Number,
          required:true
      },
      hobby:[String], // 限制字段必须是数组, 且数组内每一项都必须是字符串
      info:Schema.Types.Mixed, // 接收所有类型
      date:{
          type:Date, // 限制字段值为日期
          default:Date.now() // 默认值是Date.now()
      },
      enable_flag:{
          type:String,
          default:'Y' // 一般而言不会轻易删掉用户的信息, 如果一个用户注销掉了自己的账户, 那么就将这个值修改为N
      }
  })
  ```

#### 模式类型

- 通过type属性来设置字段的类型, 有: `String`,`Number`, `Date`, `Buffer`, `Boolean`, `Mixed`, `ObjectId`, `Array`, `Decimal128`

  - ```js
    var schema = new Schema({
      name:    String,
      binary:  Buffer,
      living:  Boolean,
      updated: { type: Date, default: Date.now },
      age:     { type: Number, min: 18, max: 65 }, // 对于Number类型, 可以设置最小最大值
      mixed:   Schema.Types.Mixed, // 允许任何允许的类型
      _someId: Schema.Types.ObjectId, // 
      decimal: Schema.Types.Decimal128, // 
      array:      [],
      ofString:   [String], // 数组, 且数组的每一项都必须是字符串
      ofNumber:   [Number], // 数组, 且数组的每一项都必须是数值
      ofDates:    [Date], // 数组, 且数组的每一项都必须是DATE
      ofBuffer:   [Buffer], // 数组, 且数组的每一项都必须是Buffer
      ofBoolean:  [Boolean], // 数组, 且数组的每一项都必须是布尔值
      ofMixed:    [Schema.Types.Mixed], // 数组, 且数组的每一项可以是任意类型
      ofObjectId: [Schema.Types.ObjectId],
      ofArrays:   [[]], // 数组, 且数组的每一项都必须是数组
      ofArrayOfNumbers: [[Number]], // 数组, 且数组的每一项都必须是数组, 且嵌套数组的每一项也必须是数值
      nested: {
        stuff: { type: String, lowercase: true, trim: true }
      }
    })
    ```

- 以String类型为例

  - ```js
    test:{
        type:String,
        lowercase:true // 总是将test转为小写
    }
    ```

  - `lowercase`选项只作用于字符串

##### 可作用于全部类型的属性

  - ```
    required:布尔值或函数 如果值为真，为此属性添加 required 验证器
    default: 任何值或函数 设置此路径默认值。如果是函数，函数返回值为默认值
    select: 布尔值 指定 query 的默认投影
    validate: 函数 为此属性添加验证器函数
    get: 函数 使用 Object.defineProperty() 定义自定义 getter
    set: 函数 使用 Object.defineProperty() 定义自定义 setter
    alias: 字符串 仅mongoose >= 4.10.0。 为该字段路径定义虚拟值 gets/sets
    ```

##### 索引相关

  - `index`: 布尔值, 是否对这个值创建索引
  - `unique`: 布尔值, 是否对这个属性创建唯一索引
  - `sparse`: 布尔值 是否对这个属性创建稀疏索引

##### 各个类型的特殊属性

###### String

- `lowercase`: 布尔值 是否在保存前对此值调用 `.toLowerCase()`
- `uppercase`: 布尔值 是否在保存前对此值调用 `.toUpperCase()`
- `trim`: 布尔值 是否在保存前对此值调用 `.trim()`
- `match`: 正则表达式 创建[验证器](http://mongoosejs.net/docs/validation.html)检查这个值是否匹配给定正则表达式
- `enum`: 数组 创建[验证器](http://mongoosejs.net/docs/validation.html)检查这个值是否包含于给定数组

###### Number

- `min`: 数值 创建[验证器](http://mongoosejs.net/docs/validation.html)检查属性是否大于或等于该值
- `max`: 数值 创建[验证器](http://mongoosejs.net/docs/validation.html)检查属性是否小于或等于该值

##### Date

- `min`: Date
- `max`: Date

- 剩下的不写了, 如果出现问题直接去文档里查

### 使用架构创建模型对象

- 模型对象是从Schema编译来的构造函数. 它们的实例就代表着可以从数据库保存和读取的documents. 从数据库创建和读取 document 的所有操作都是通过 model 进行的。

- ```js
  let myModel=mongoose.model('student',myRule) // 参数一: 该模型对应的表名, 参数二: 应用的架构
  ```

- 第一个参数是跟 model 对应的集合（ collection ）名字的 单数 形式。 Mongoose 会自动找到名称是 model 名字 复数 形式的 collection 

  - myModel这个模型就对应数据库中`students`这个collection.
  -  你要确保在调用 `.model()` 之前把所有需要的东西都加进 `schema` 里了

- 

### C_创建数据

- ```js
  myModel.create({
      stu_id:'001',
      age:16,
      hobby:['xx','yyy'],
      info:666
  },function(err,data){
      if(err) return console.log(err)
      consoel.log(data)
  })
  ```

- 或者根据模型对象new创建

  - ```js
    var instance=new myModel({
        stu_id:'001',
        age:16,
        hobby:['xx','yyy'],
        info:666
    })
    instance.save(function(err){
        if(err) return handleError(err)
    }) // 可以链式连写
    ```

  - 




### R_查询

#### 基本查询

- find()

  - 返回一个数组, 没有数据返回空数组

  - ```js
    myModel.find({name:'xx'},function(err,data){
        if(err) return console.log(err)
        console.log(data)
    })
    ```
  
- findOne

  - 只返回一个对象, 即最先找到的那个对象, 用法与find一致, 如果没有查询到任何结果, 返回null

#### 获取查询对象的指定值

- ```js
  myModel.findOne({name:'xx'},{age:1,_id:0},(err,data)=>{
      if(err) return console.log(err)
      console.log(data) // {age:16}
  }).limit
  ```

- 第二个对象代表返回的值中只包含age属性, 不包含_id属性, 其它属性没有指定则不返回

  - 为什么要指定{_id:0}? 因为除非你指定不返回它, 默认会返回`_id`值

### U_更新







### 遇见的一些错误

- `schematype.castForQueryWrapper is not a function`
  - 有一个Model, 在测试里使用`model.findOne()`可以查找到数据, 但在服务器里使用`model.findOne()`报如上错误
  - 

# vue全家桶

## vue2.x

### class与style绑定



#### 绑定内联样式

- 对象语法

  - ```vue
    <div :style="{color:activeColor,fontSize:myFontSize+'px'}">
        
    <!--分隔-->
    data:{
        activeColor:'red',
        myFontSize:15
    }
    ```

  - css属性名可以使用驼峰式或短横线分割

  - 或者直接绑定一个对象

    - ```vue
      <div v-bind:style="styleObject"></div>
      
      <!--分隔-->
      data: {
        styleObject: {
          color: 'red',
          fontSize: '13px'
        }
      }
      ```

### 数组语法

- 将多个已经设定好的对象样式应用到同一个元素上

  - ```vue
    <div v-bind:style="[baseStyles, overridingStyles]"></div>
    ```





### 条件渲染

#### v-if

#### v-show

- 与v-if不同的是, v-show的元素会始终被渲染并保留在DOM中. v-show只是简单地切换该元素的css属性的display

#### v-if与v-show

- v-if才是真正的"条件渲染"

- 如果一开始v-if的值就为false的话, 那么直到值变为true时, v-if的元素才会第一次被渲染
- 不管v-show的值为什么, 元素一开始就会被渲染, 并且基于display进行切换
- v-if有更高的切换开销, v-show有更高的初始渲染开销. 因此频繁切换使用v-show较好, 运行时条件很少改变, 则v-if较好







## vue-router@3.5.3

- 只有3版本才适配vuejs2.x

### 1 安装

- `npm i vue-router`

### 2 基本使用

- 2.1 在html文件中, 使用`<router-link to="/foo"></router-link>`来作为链接, 使用`<router-view></router-view>`来作为模板位置标识

  - ```js
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
    
    <div id="app">
      <h1>Hello App!</h1>
      <p>
        <!-- 使用 router-link 组件来导航. -->
        <!-- 通过传入 `to` 属性指定链接. -->
        <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
        <router-link to="/foo">Go to Foo</router-link>
        <router-link to="/bar">Go to Bar</router-link>
      </p>
      <!-- 路由出口 -->
      <!-- 路由匹配到的组件将渲染在这里 -->
      <router-view></router-view>
    </div>
    ```

- 2.2 在js文件中

  - ```js
    // 1 导入Vue与VueRouter(如果没导入过的话)
    // 2 使用VueRouter插件
    Vue.use(VueRouter)
    
    // 3 定义路由和对应组件
    const routes=[
        {path:'/foo',component:Foo},
        {path:'/bar',component:Bar}
    ]
    
    // 4 创建VueRouter实例
    const router=new VueRouter({
        routes
    })
    
    // 5 创建和挂载根实例
    const app=new Vue({
        router
    }).$mount('#app')
    ```

### 3 动态路由匹配

- 我们经常需要把某种模式匹配到的所有路由，全都映射到同个组件。例如，我们有一个 `User` 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染。

- ```js
  const router = new VueRouter({
    routes: [
      // 动态路径参数 以冒号开头
      { path: '/user/:id', component: User }
    ]
  })
  ```

- `user/foo`与`user/bar`都将映射到相同的路由

- 路径匹配时, 路径参数的参数值会被保存在 `this.$route.params` , 可以在任何组件中使用.

  - 比如上面的路径参数 `id` , 通过 `this.$route.params.id`来获取值

- 允许设置多个路径参数

  - ```js
    /user/:username/post/:post_id
    ```

    - 上面的动态路由可以匹配 `/user/evan/post/123`, 打印 `this.$route.params` 获得:

      - ```js
        { username: 'evan', post_id: '123' }
        ```

- 除了`this.$route.params`来捕获路由参数外, 还有`this.$route.query`来捕获URL中的查询参数, `this.$route.hash`, 详细查看路由对象

#### 3.1 响应路由参数的变化

- 从`user/foo`到`/user/bar`, 组件实例会被复用, 组件的生命周期钩子不会再被调用??????????

- **我必须在这里注释我的测试结果: **

  - 当我使用动态路由匹配从`/user/foo`向`/user/bar`跳转时, user的`mounted`生命周期钩子函数是被调用的, 在服务器中也清晰地看到服务器被访问了两次, 所以我觉得文档里写的这个是错误的 
  - 当我注释掉 routes/index.js 文件中的`mode:history`后, 就不再调用生命周期函数了, 如果将`mode:history`添加上, 那么只要路径改变, 就会刷新整个页面, 并重新调用生命周期钩子函数

- 这一份文档里唯一有用的是给出如何监测路由参数的变化:

  - 复用组件时, 想对路由参数的变化作出响应的话, 简单地watch`$route`对象

  - ```js
    const User={
        template: '...',
        watch:{
            $route(to,from){
                // 对路由变化作出响应...
                // 在to.params和from.params中可以查看动态路由所匹配的路径字符串
            }
        }
    }
    ```

  - 或者使用`beforeRouteUpdate`导航守卫:

    - ```js
      const User={
          template:'...',
          beforeRouteUpdate(to,from,next){
              
          }
      }
      ```

#### 3.2 捕获所有路由或 404 Not found 路由

- ```js
  {
    // 会匹配所有路径
    path: '*'
  }
  {
    // 会匹配以 `/user-` 开头的任意路径
    path: '/user-*'
  }
  ```

- 当使用一个*通配符*时，`$route.params` 内会自动添加一个名为 `pathMatch` 参数。它包含了 URL 通过*通配符*被匹配的部分：

  - ```js
    // 给出一个路由 { path: '/user-*' }
    this.$router.push('/user-admin')
    this.$route.params.pathMatch // 'admin'
    // 给出一个路由 { path: '*' }
    this.$router.push('/non-existing')
    this.$route.params.pathMatch // '/non-existing'
    ```

#### 3.3 高级匹配模式

#### 3.4 匹配优先级

有时候，同一个路径可以匹配多个路由，此时，匹配的优先级就按照路由的定义顺序：路由定义得越早，优先级就越高。





## 嵌套路由(子路由)

children属性

- ```js
  const router = new VueRouter({
    routes: [
      {
        path: '/user/:id',
        component: User,
        children: [
          {
            // 当 /user/:id/profile 匹配成功，
            // UserProfile 会被渲染在 User 的 <router-view> 中
            path: 'profile',
            component: UserProfile
          },
          {
            // 当 /user/:id/posts 匹配成功
            // UserPosts 会被渲染在 User 的 <router-view> 中
            path: 'posts',
            component: UserPosts
          }
        ]
      }
    ]
  })
  ```

- **要注意，以 `/` 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。**

- 此时，基于上面的配置，当你访问 `/user/foo` 时，`User` 的出口是不会渲染任何东西，这是因为没有匹配到合适的子路由。如果你想要渲染点什么，可以提供一个 空的 子路由：

  - ```js
    const router = new VueRouter({
      routes: [
        {
          path: '/user/:id',
          component: User,
          children: [
            // 当 /user/:id 匹配成功，
            // UserHome 会被渲染在 User 的 <router-view> 中
            { path: '', component: UserHome }
    
            // ...其他子路由
          ]
        }
      ]
    })
    ```

### 编程式导航

- `router.push(location, onComplete?, onAbort?)`
- 在 Vue 实例内部，你可以通过 `$router` 访问路由实例。因此你可以调用 `this.$router.push`
- 当你点击 `<router-link>` 时，这个方法会在内部调用，所以说，点击 `<router-link :to="...">` 等同于调用 `router.push(...)`



#### 遇见的错误记录

##### NavigationDuplicated: Avoided redundant navigation to current location: "/login"

- 翻译: 避免重复导航到当前位置:“/login”。
- 原因是因为从 /login 访问 /login , 报错
- 解决方法: 检查当前路径
  - 



##### 页面渲染最先走的是路由的钩子

- 当用户把本地存储全部清空以后，再刷新的时候，页面渲染最先走的是路由的钩子，而不是vue组件的钩子（create之类的），路由的钩子放行next()以后，才会继续往下执行，才会进入对应vue组件的create、mounted钩子，才会向后端发请求。但是因为我们使用beforeEach路由全局钩子做了拦截，所以当sessionStoarge没有token的时候，就会自动跳转到登录页了。所以不会报错.

## 重定向和别名

### 重定向

- `https://www.cnblogs.com/phoebeyue/p/9257705.html`

- ```js
  const router = new VueRouter({
   	routes: [
   		{ path: '/a', redirect: '/b' }
   	]
  })
  ```

- 跳转一个命名路由

  - ```js
    //重定向的目标也可以是一个命名的路由
    const router = new VueRouter({
     	routes: [
     		{ path: '/a', redirect: { name: 'foo' }}
     	]
    })
    ```

#### redirect动态跳转路由

- ```js
  	{
  		path: '/dynamic-redirect/:id?',
  		redirect: to => {
  			const { hash, params, query } = to
  			if (query.to === 'foo') {
  				return { path: '/foo', query: null }
  			}
  			if (hash === '#baz') {
  				return { name: 'baz', hash: '' }
  			}
  			if (params.id) {
  				return '/with-params/:id'
  			} else {
  				return '/bar'
  			}
  		}
  	}
  ```

- redirect 不可以写 async/await 函数



## HTML5 History 模式

> `vue-router` 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

> 如果不想要很丑的 hash，我们可以用路由的 **history 模式**，这种模式充分利用 `history.pushState` API 来完成 URL 跳转而无须重新加载页面。

```JS
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

- 在hash模式下, 浏览器对于所有路径都是访问 `http://192.168.1.4:8081/`这种根目录( 因为是单页面 ), 所以在采用history模式的路径中, 刷新会直接访问服务器的对应路由.

- 所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 `index.html` 页面，这个页面就是你 app 依赖的页面

- 开发环境的devServer配置

  - 在webpack的devServer中添加

    - ```js
      {historyApiFallback:true}
      ```

    - 这个设置将任意的404响应都替换为index.html

- 同时我注意到, history模式下, 动态路由的改变会直接刷新页面, 不会复用组件





[面试官：说说前端路由与后端路由的区别](https://juejin.cn/post/7023932512363610120)

### 导航守卫

#### 全局前置守卫

- ```js
  const router = new VueRouter({ ... })
  
  router.beforeEach((to, from, next) => {
    // ...
  })
  ```

- 每个守卫方法接收三个参数：

  - **`to: Route`**: 即将要进入的目标 [路由对象](https://v3.router.vuejs.org/zh/api/#路由对象)
  - **`from: Route`**: 当前导航正要离开的路由
  - **`next: Function`**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。
    - **`next()`**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed** (确认的)。
    - **`next(false)`**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
    - **`next('/')` 或者 `next({ path: '/' })`**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 [`router-link` 的 `to` prop](https://v3.router.vuejs.org/zh/api/#to) 或 [`router.push`](https://v3.router.vuejs.org/zh/api/#router-push) 中的选项。
    - **`next(error)`**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 [`router.onError()`](https://v3.router.vuejs.org/zh/api/#router-onerror) 注册过的回调

- **确保 `next` 函数在任何给定的导航守卫中都被严格调用一次。它可以出现多于一次，但是只能在所有的逻辑路径都不重叠的情况下，否则钩子永远都不会被解析或报错**。这里有一个在用户未能验证身份时重定向到 `/login` 的示例：

  - ```js
    router.beforeEach((to, from, next) => {
      if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
      else next()
    })
    ```

  - 在全局前置守卫中跳转路由, 必须检查to的路由, 否则容易造成死循环

#### 路由独享的守卫

- 通过直接在路由配置上定义`beforeEnter`守卫

  - ```js
    const routes = [
      {
        path: '/users/:id',
        component: UserDetails,
        beforeEnter: (to, from) => {
          // reject the navigation
          return false
        },
      },
    ]
    ```

  - > `beforeEnter` 守卫 **只在进入路由时触发**，不会在 `params`、`query` 或 `hash` 改变时触发。例如，从 `/users/2` 进入到 `/users/3` 或者从 `/users/2#info` 进入到 `/users/2#projects`。它们只有在 **从一个不同的** 路由导航时，才会被触发。(上面的几个案例是不会触发)

- 也可以将一个函数数组传递给 `beforeEnter`，这在为不同的路由重用守卫时很有用：

  - ```js
    function removeQueryParams(to) {
      if (Object.keys(to.query).length)
        return { path: to.path, query: {}, hash: to.hash }
    }
    
    function removeHash(to) {
      if (to.hash) return { path: to.path, query: to.query, hash: '' }
    }
    
    const routes = [
      {
        path: '/users/:id',
        component: UserDetails,
        beforeEnter: [removeQueryParams, removeHash],
      },
      {
        path: '/about',
        component: UserDetails,
        beforeEnter: [removeQueryParams],
      },
    ]
    ```

  - 也可以使用路径 meta字段 和全局导航守卫来实现类似的行为

##### 补充通过beforeEnter守卫来动态跳转路由

- ```js
  	{
  		path: '/',
  		beforeEnter(to, from, next) {
  			// 如果携带有token的话, 执行自动登录逻辑, 进入主页/msite
  			// 如果没有携带token的话, 跳转/login
  			const token = localStorage.getItem('token_key')
  			if (token) {
  				console.log('来自路由的判断: 本地存在token, 前往/msite')
  				next({ path: '/msite' })
  			} else {
  				console.log('来自路由的判断: 本地不存在token, 前往/login')
  				next({ path: '/login' })
  			}
  		}
  	}
  ```

- 说明: 对于根路由的访问, 如果存在本地token的话, 前往/msite, 如果不存在本地token的话, 前往/login





#### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫(2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫(2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。



## 报错问题积累

### [vue-router] "path" is required in a route configuration.

- 这个问题一般都是routes.js, 也就是路由规则出错, 应该检查路由规则

# npm插件

## 1. axios

### 拦截器

```js
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error); // 请求拦截器的错误会被响应拦截器拦截
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```



### 多个拦截器的顺序

```js
const axios = require('axios')

const instance = axios.create({
	baseURL: 'http://127.0.0.1:3058',
	timeout: 3000
})

instance.interceptors.request.use(
	config => {
		console.log('请求拦截器1')
		return config
	},
	error => {
		return new Promise(() => {})
	}
)

instance.interceptors.request.use(
	config => {
		console.log('请求拦截器2')
		return config
	},
	error => {
		return new Promise(() => {})
	}
)

instance.interceptors.response.use(
	response => {
		console.log('响应拦截器1')
		return response
	},
	error => {
		return new Promise(() => {})
	}
)

instance.interceptors.response.use(
	response => {
		console.log('响应拦截器2')
		return response
	},
	error => {
		return new Promise(() => {})
	}
)

instance.get('/test1').then(
	value => {
		console.log(value.data)
	},
	reason => {}
)

// 请求拦截器2
// 请求拦截器1
// 响应拦截器1
// 响应拦截器2
// success from /test1
```

- 请求拦截器会从最后一个向文本上方的请求拦截器传递config
- 响应拦截器会从第一个响应拦截器向文本最后一个响应拦截器传递response

- 如果中途某个拦截器报错

  - ```js
    instance.interceptors.request.use(
    	config => {
    		console.log('222222')
    	},
    	error => {
    		console.log('33333') // 首先进入第二个请求拦截器的错误链中
    		return Promise.reject('请求拦截器报错')
    	}
    )
    
    instance.interceptors.request.use(
    	config => {
    		throw new Error('来自请求拦截器的报错')
    	},
    	error => {}
    )
    
    instance.interceptors.response.use(
    	response => {
    		console.log('11111')
    	},
    	error => {
    		console.log(error)
    		return Promise.reject('请求报错')
    	}
    )
    instance.get('/test1').then(
    	value => {
    		console.log(value.data)
    	},
    	reason => {
    		console.log(reason)
    	}
    )
    ```

  - 



### 错误处理

```js
axios.get('/user/12345')
  .catch(function (error) {
    if (error.response) {
      // 请求发出后，服务器用状态码响应
      // 状态码不在2xx的范围内
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else if (error.request) {
      // 提出了请求，但没有收到答复
      // `error.request` 是浏览器的XMLHttpRequest的实例和nodejs中http.clientRequest的实例
      console.log(error.request);
    } else {
      // 在设置请求时发生了一些事情，触发了一个Error
      // 比如在请求拦截器中抛出错误, 就只有一个error对象
      console.log('Error', error.message);
    }
    console.log(error.config);
  });
```

- 收到的请求的状态码不在2xx范围内使用err.response接收错误
  - err.response对象包括 `data`,`status`,`headers`三个属性
- 成功发送请求但是服务器没有给予答复使用err.request接收错误
  - 它在浏览器中是XMLHttpRequest的实例, 大概有readyState的属性之类的吧(我猜的)
- 其它错误通过error.message来获取

## 2.babel

- 安装

  - ```
    npm install --save-dev @babel/core @babel/cli @babel/preset-env
    ```

- 配置babel.config.json

  - 先在当前目录下创建一个babel.config.json文件

  - ```json
    {
      "presets": [
        [
          "@babel/preset-env",
          {
            "targets": {
              "edge": "17",
              "firefox": "60",
              "chrome": "67",
              "safari": "11.1"
            },
            "useBuiltIns": "usage",
            "corejs": "3.6.5"
          }
        ]
      ]
    }
    ```

- 编译代码

  - 从一个目录编译到另一个目录( 从 `src` 目录编译到 `lib` )

    - ```
      ./node_modules/.bin/babel src --out-dir lib
      ```

    - ```
      npx babel src --out-dir lib
      ```

### babel-cli

- 安装

  - ```
    npm install --save-dev babel-cli
    ```

- 基本用法

  - ```
    babel script.js
    ```



#### babel-node

- 编译并运行js文件

  - ```js
    // 编译并运行 test.js
    npx babel-node test
    ```

  - 



## vuex3.x

- 只有vuex3.x适配vue2.x





### 如何在通过引入





## jsonwebtoken

- 安装

  - `npm i jsonwebtoken`

- 使用

  - ```js
    jwt.sign(payload, secretOrPrivateKey, [options,callback])
    ```

  - payload 可以是表示有效 JSON 的对象文字、Buffer或String。

  - secretOrPrivateKey 是一个字符串、缓冲区或对象

  - options: 

    - algorithm : 默认HS256,
    - expiresIn: 以秒或描述时间跨度 zeit/ms 的字符串表示, 可以是 60(即'60ms'), '2 days', '10 h' , '7 d'

- 创建token

  - ```js
    const jwt=require('jsonwebtoken')
    module.exports=function(id){
        // 根据用户的id生成token
        // 为方便测试，有效期设置为 10s 进行监测，普通生产情况下可以设置为更长的时间 
        const token = jwt.sign({id},'secret',{ expiresIn: '7 days'})
    }
    ```

- 验证token

  - ```js
    const jwt = require('jsonwebtoken')
    
    module.exports = function (req, res, next) {
    
    	// 如果没有authorization请求头, 直接返回401响应
    	const token = req.headers['authorization']
    	if (!token) {
    		res.status(401)
    		return res.json({ message: '无token，请重新登录' })
    	}
    	// 解构 token，生成一个对象 { id: xx, iat: xx, exp: xx }
    	let decoded = jwt.decode(token, 'secret')
    	// 如果token已过了有效期, 返回401响应
    	if (!decoded || decoded.exp <= Date.now() / 1000) {
    		res.status(401)
    		return res.json({ message: 'token过期，请重新登录' })
    	}
    
    	// 通过了token验证, 放行
    	next();
    }
    ```

- 





# 其它一些暂时找不到分类的集合

## token

- `https://www.cnblogs.com/ahaMOMO/p/12373287.html`
- `https://www.jianshu.com/p/1422374a404b`

### 什么是token

- token的意思是“令牌”，是服务端生成的一串字符串，作为客户端进行请求的一个标识。
- 当用户第一次登录后，服务器生成一个token并将此token返回给客户端，以后客户端只需带上这个token前来请求数据即可，无需再次带上用户名和密码。
- 简单token的组成；uid(用户唯一的身份标识)、time(当前时间的时间戳)、sign（签名，token的前几位以哈希算法压缩成的一定长度的十六进制字符串。为防止token泄露）。

### 为什么要用token

- Token 完全由应用管理，所以它可以避开同源策略
- Token 可以避免 CSRF 攻击
- Token 可以是无状态的，可以在多个服务间共享

### 基于token机制的身份认证

使用token机制的身份验证方法，在服务器端不需要存储用户的登录记录。大概的流程：

1. 客户端使用用户名和密码请求登录。
2. 服务端收到请求，验证用户名和密码。
3. 验证成功后，服务端会生成一个token，然后把这个token发送给客户端。
4. 客户端收到token后把它存储起来，可以放在cookie或者Local Storage（本地存储）里。
5. 客户端每次向服务端发送请求的时候都需要带上服务端发给的token。
6. 服务端收到请求，然后去验证客户端请求里面带着token，如果验证成功，就向客户端返回请求的数据。(如果这个 Token 在服务端持久化（比如存入数据库），那它就是一个永久的身份令牌。)

### JWT

- 实施 Token 验证的方法挺多的，还有一些标准方法，比如 JWT，读作：jot ，表示：JSON Web Tokens 。JWT 标准的 Token 有三个部分：

- ```
  1.header(头部)，头部信息主要包括（参数的类型--JWT,签名的算法--HS256）
  2.poyload(负荷)，负荷基本就是自己想要存放的信息(因为信息会暴露，不应该在载荷里面加入任何敏感的数据)
  3.sign(签名)，签名的作用就是为了防止恶意篡改数据，下边会详细说明
  ```

- 中间用点分隔开，并且都会使用 Base64 编码，所以真正的 Token 看起来像这样：

  - ```
    eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJuaW5naGFvLm5ldCIsImV4cCI6IjE0Mzg5NTU0NDUiLCJuYW1lIjoid2FuZ2hhbyIsImFkbWluIjp0cnVlfQ.SwyHTEx_RQppr97g4J5lKXtabJecpejuef8AqKYMAJc
    ```



## 视频课的解释

- ```
  1). 作用
      a. 是一个包含特定信息的字符串:　id / 失效的时间
      a. 对请求进行一定的检查限制, 防止恶意请求
      b. 后台部分接口需要进行token验证  ==> 只有请求这些接口时才携带token
  2). 使用流程
      a. 客户端发送登陆的请求, 服务器端进行用户名和密码查询, 
          如果user存在, 根据user的id值生成token(指定了有效期), 将user和token返回给客户端
      b. 客户端接收到登陆成功的响应后, 将token保存localStorage, 将user和token保存在vuex的state
      c. 在请求需要授权检查的接口前(在请求拦截器做)
          如果token不存在, 不发请求, 直接进行错误流程(响应拦截器的错误处理): throw error对象(status: 401)
          如果token存在, 将token添加到请求头中: config.headers.Authorization = token
      d. 在响应拦截器中处理错误
          1). 如果error中没有response
              判断error的status为401, 如果当前没有在登陆页面, 跳转到登陆页面
          2). 如果error中有response, 取出response中的status
              status为: 401: token过期了, 退出登陆(清除local中的token和state中user与token), 并跳转到登陆页面
              status为: 404: 提示访问的资源不存在
  ```





# other 其它学习过程中解决的问题

## other.1 switchHosts没有写入hosts文件的权限

- 没有解决, 只解决了获取最新github的dns地址的问题; 依然手动修改dns

```
https://gitlab.com/ineo6/hosts
```

# last 不重要但是花费了时间的东西



## last.1 powershell的美化

​	放弃. 我为什么要花时间在PC的其它不重要方面? 我的PC是工具, 伐木工人会在斧头或者电锯上贴可爱的贴纸吗? 对于工具来说, 实用和简约才是第一追求目标, 美化是在此之外的东西.



