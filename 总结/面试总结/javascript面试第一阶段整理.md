[Toc]

# 语言基础

## 语法

### 严格模式与非严格模式的区别

- 对语句的限制：

  - 1.严格模式不允许使用with。

  - 2.严格模式下变量必须声明，赋值给未声明的变量会报错，js引擎不会在严格模式下隐式创建全局变量。

  - 3.严格模式下的eval有自己的词法作用域，其中的声明无法再修改所在作用域的词法作用域

    - ```
          // 非严格模式下
          function fn(str, a) {
              eval(str)
              console.log(a, b)
          }
          var b = 2
          fn("var b=3", 1) // 1,3
      ```

    - ```
          'use strict'
        
          function fn(str, a) {
              eval(str)
              console.log(a, b)
          }
          var b = 2
          fn("var b=3", 1) // 1,2
      ```

  - 4.严格模式下删除或修改{configurable:false}的属性时报错，而不是忽略。

- 对函数的限制：

  - 1.函数不能以eval，argumentsWie函数名或参数名
  - 2.函数的参数名不可一样。
  - 3.严格模式的arguments是静态副本，修改arguments不会动态修改实参的值。
  - 4..arguments.callee，arguments.caller被禁用

- 对赋值的限制：

  - 严格模式下禁止使用8进制字面量。

    - ```
          let a = 010
          console.log(a) // Octal literals are not allowed in strict mode
      ```

- 对this的限制

  - 严格模式下this的默认绑定不再自动绑定到widnow对象，而是undefined。显式绑定时如果传入null或undefined，同样采用默认绑定规则，但是多了一个null，绑定至null或者undefined。

## 变量

### var\let\const的使用和区别

- var
  - js引擎会预解析var声明的变量，作为window的属性。并对var声明的变量进行提升。
- let
  - let声明的变量不会被提升
  - 重复的var声明会被忽略，但重复的let声明会报错。
  - 全局执行上下文中let声明的变量不会成为window的属性。
- const
  - const定义的是常量，不允许修改，不允许只声明不赋值，同样只作用于块级作用域。
  - const声明的对象的属性可以被修改或删除，不受限制。
  - 整个对象都不允许修改的话使用`Object.freeze()`

## 数据类型

### typeof判断数据类型上的差异

### 手写typeof

### 数据类型的显示转换与隐式和转换

## 操作符

### 位操作的原理与正数，负数的二进制编码计算

- 原理：在ECMAScript中，数值以64位保存，以32位操作。其中第32位是符号位（sign bit，0表示整数，1表示负数）。
- 正值的二进制编码（以18为例）：`0...(27个0)10010`
  - 取余法：
    - 18/2=9...0
    - 9/2=4....1
    - 4/2=2...0
    - 2/2=1...0（1已是最后的结果）
    - 取最后结果与所有余数：10010
- 负值以二补数的二进制编码储存（以-18为例）：
  - 首先求负数的绝对值的二进制，即18的二进制
  - 再求这个二进制数的一补数，即1变为0,0变为1：`1...(27个1)01101`
  - 再将一补数加1:`1...(27个1)01110`

### 按位非

- 使用`~`表示，返回数值的一补数。在十进制层面上表示为对数值取反并减1
- 以18为例：`0...(27个0)10010`
  - 取一补数：`1...(27个1)01101`
  - 将这个数换算成十进制：识别二进制的十进制为负数，首先二进制数减一得到`1...(27个1)01100`
  - 再求负值的绝对值：`0...(27个0)10011`得到19
  - 因为是负数：得到-19

### 按位与

- 使用`&`表示，将两个数的32位二进制数一一对齐。依照与运算，全1得1，其它都得0
- ![image-20211021113146834](C:\Users\董磊\AppData\Roaming\Typora\typora-user-images\image-20211021113146834.png)

### 按位或

- 使用`|`表示，将两个数的32位二进制数一一对齐。依照或运算，有1得1，全0得0
- ![image-20211021113356767](C:\Users\董磊\AppData\Roaming\Typora\typora-user-images\image-20211021113356767.png)

### 按位异或

- 使用`^`表示，将两个数的32位二进制数一一对齐。依照异或运算，只有一位上是1时得1，全1或全0时得。
- ![image-20211021114417260](C:\Users\董磊\AppData\Roaming\Typora\typora-user-images\image-20211021114417260.png)

## 语句

### with语句的用法及缺点

# 变量、作用域与内存

## 原始值与引用值

### 简单数据类型与引用数据类型的储存与复制有什么区别

### 浅复制与深复制

### 如何理解函数中的参数都是按值传递的

### instanceof判断数据类型与typeof有何区别

- typeof用于确定值的原始类型，instanceof用于确定值的引用类型

### 手写instanceof

## 执行上下文与作用域

### 解释执行上下文

- 执行上下文分为全局上下文、函数上下文和块级上下文
- 代码执行流每进入一个新的

### 解释作用域以及作用域链

- 上下文决定的是变量或函数可以访问哪些数据
- 每个上下文都有一个关联的变量对象(VO)，这个上下文中定义的所有变量和函数都存在于这个对象上。
- 上下文的在其所有代码都执行完毕后会被销毁，包括定义在它上面的变量和函数
- 每个函数都有自己的函数上下文，当代码执行流进入到函数中时，其上下文被推入到一个上下文栈中。函数执行完毕弹出该上下文。
- 上下文中的代码在执行时会创建一个作用域链。代码正在执行的上下文的变量对象始终位于作用域链的最前端。如果上下文是函数，那么活动对象(AO)用作变量对象。全局上下文的变量对象始终是作用域链上的最后一个对象。
- 标识符查询从作用域链的最前端开始查起，直到查到作用域链的底部（全局执行上下文的变量对象）

### 解释声明的提升(hoisting),var,let,const有何不同

- var声明

### 解释js的垃圾回收机制

- 离开作用域的值会被自动标记为可回收，在垃圾回收期间被删除
- 主流的垃圾回收算法是标记清理，即先给当前不使用的值加上标记，再回收它们的内存。

- 如何主动回收掉垃圾？
  - 解除引用：将对象的值设置为null
  - 闭包函数在调用后会返回一个函数赋值给其它作用域的变量。因此会造成已经没有使用的作用域的变量对象被保存下来。

### 内存管理如何提升性能

- 使用const和let提升性能
  - 块作用域比函数作用域更早结束。因此相比于var，它们会使得垃圾回收机制更早介入。
- 使用构造函数创建对象，避免“先创建后补充”（创建后再对属性进行添加或删除）
  - 使用同一个构造函数创建的两个实例对象，它们共享一个隐藏类（因为它们共享同一个构造函数和原型）
  - 添加或删除实例对象的属性会造成其对应不同的隐藏类
  - 将不想用的属性设置为null会保持隐藏类不变。

### 解释js内存泄漏

- 哪些情况会造成内存泄漏

  - 不带var，let，const就赋值变量，造成声明全局变量

  - setInterval()等定时器中的回调函数不通过传参而是标识符查找外部词法作用域中的变量造成该变量一直被引用。

    - ```
      let name="jake"
      setInterval(()=>{
      	consoel.log(name)
      },1000)
      ```

  - 闭包函数造成内存泄漏

    - ```
      function outer(){
      	let name="jake"
      	return function(){
      		return name
      	}
      }
      ```

### 解释js的编译原理

- 分词/词法分析
  - 将代码的字符分解成有意义的代码块。。这些代码块被称为词法单元(token)



## 四种this的绑定方式

- 默认绑定

  - 通过`fn()`这种方式调用的就是默认绑定。默认绑定在非严格模式下绑定到window对象上。

- 隐式绑定

  - 通过`obj.fn()`方式绑定的就是隐式绑定。

  - 隐式丢失：

    - 

    ```javascript
            let obj = {
                fn: function () {
                    console.log(this)
                }
            }
    
            setTimeout(obj.fn, 0); // window
    ```

    - setTimeout函数相当于

    - 

    ```php
            function setTimeout(fn, delay) {
                // 等待delay秒后
                fn()
            }
    ```

- 显式绑定

  - 通过`fn.call(obj)`就是显式绑定，除此之外还有apply，bind
    - call与apply的区别是call的参数是通过逗号间隔的字符串，apply是数组
  - 将null或者undefined作为this的绑定对象传入call，apply或者bind，这些值在调用时会被忽略掉。应用默认绑定规则。

- new绑定

  - 通过`obj = new fn()`方式调用。this绑定在obj上。

## 调用new发生了什么

- 创建一个空对象
- 将this值指向这个空对象
- 执行函数
- 将执行结果返回

## call，apply，bind的区别

## 手写bind方法

## 

## 手写instanceof方法

## 简要介绍包装对象是什么

## 字符串的ES5方法有哪些，ES6方法有哪些？

## 数组的ES5方法有哪些，ES6方法有哪些？



## innerHTML，innerText，textContent的区别

## onmouseover与onmouseenter的区别，onmouseout与onmouseleave的区别

## offsetWidth / offsetHeight / offsetLeft / offsetTop / offsetParent

- `offsetWidth`获取元素的`width + padding + boder`的值
  - 如果一个元素设置为`box-sizing:border-box;`那么`width:200px`的元素的`offsetWidth`不管怎么设置都是200px。
- `offsetHeight`获取元素的`height + padding + border`的值，参上。
- `offsetLeft`/`offsetTop`获取相对于包围元素的偏移，即左边距，右边距。
- `offsetParent`是相对于包含元素的，但是包含元素不一定是`parentNode`
  - 比如`td`的相对于`table`偏移，因为table是节点层级中第一个提供尺寸的。
  - 比如我写两个包含div，没有进行任何定位，里面的那个div的`offsetParent`是`body`。

## 箭头函数和普通函数的区别

## TCP和UDP的区别

## javascript垃圾回收方法

## web storage和cookie的区别

## 实现继承的方式

### 原型链继承

### 借用构造函数继承

### 组合继承

## 讲一讲http

## 手写promise

## 请描述一下 cookies sessionStorage和localstorage区别

- 相同点：都存储在客户端

- 不同点：

  1.存储大小

  - cookie数据大小不能超过4k。

  - sessionStorage和localStorage 虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。

  2.有效时间

  - localStorage   存储持久数据，浏览器关闭后数据不丢失除非主动删除数据；
  - sessionStorage  数据在当前浏览器窗口关闭后自动删除。
  - cookie      设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭

  3.数据与服务器之间的交互方式

  - cookie的数据会自动的传递到服务器，服务器端也可以写cookie到客户端
  - sessionStorage和localStorage不会自动把数据发给服务器，仅在本地保存。

## 原型对象与原型链

- 每一个函数都有一个prototype属性，它指向的是原型对象（显式原型对象）。么一个实例对象身上都有一个`__proto__`属性同样指向原型对象（隐式原型对象）。原型对象的本质是Object构造函数的实例对象。

- 查找对象的属性时，首先在对象本身寻找这个属性，如果不存在，则沿着`__proto__`找原型对象里是否存在这个属性。一直找到Object原型对象null为止。如果未找到，则返回undefined。沿着`__proto__`查找的这条链就是原型链。

## 原型链笔试题

### 1

```
        var A = function () {

        }
        A.prototype.n = 1
        var b = new A()

        A.prototype = {
            n: 2,
            m: 3
        }
        var c = new A()
        console.log(b.n, b.m, c.n, c.m) // 1 undefined 2 3
```

- 重写原型对象只是修改之后新建的实例对象的`__proto__`指向。但已经被创造的实例对象的`__proto__`的指向并没有改变。原来的原型对象并没有消失。

```
        var F = function () {}
        Object.prototype.a = function () {
            console.log('a()')
        }

        Function.prototype.b = function () {
            console.log('b()')
        }
        var f = new F()
        f.a() // 'a()'
        // f.b() // not a function
        F.a() // 'a()'
        F.b() // 'b()'
```

- 函数的原型对象是Object构造函数的实例，因此存在`__proto__`指向Object.prototype。函数实例是可以访问构造函数原型对象的属性的



## 正则表达式

### 基础题：分别指出`\d`,`\D`,`\w`,`\W`,`\s`,`\S`,`.`，`\b`,`\B`所匹配的项

- `\d`匹配0-9的数字
- `\D`匹配所有非数字字符。是`\d`的绝对补集（子集`\d`在全字符集合U的绝对补集）。
- `\w`匹配A-Z，a-z，0-9和下划线字符
- `\W`匹配所有不是英文字符，数字和下划线的字符。（子集`\w`在全字符集合U中的绝对补集）
- `\s`匹配空白字符，包括制表符，回车符，空格等没有显示文本的字符
- `\S`匹配非空白字符，所有有显示的字符都是它的匹配项。（子集`\s`在全字符集合U中的绝对补集。
- `.`匹配除换行符外的任意单个字符。
- `\b`匹配`\w`字符与`\W`字符之间的位置

```javascript
        let str='ada assdas saha5ass4das asda阿sada as4_asda5a'
        let reg=/\b/g
        let newStr=str.replace(reg,'|')
        document.write(newStr)
```

![image-20211022105122990](C:\Users\董磊\AppData\Roaming\Typora\typora-user-images\image-20211022105122990.png)

- `\B`匹配`\w`字符与`\w`字符之间的位置

```javascript
        let str='ada assdas saha5ass4das asda阿sada as4_asda5a'
        let reg=/\B/g
        let newStr=str.replace(reg,'|')
        document.write(newStr)
```

![image-20211022105226858](C:\Users\董磊\AppData\Roaming\Typora\typora-user-images\image-20211022105226858.png)

- 快速记忆：`\d`匹配0-9，`\w`匹配A-Z，a-z，0-9和下划线，`\s`匹配所有空白字符，比如换行符。所有大写字母均是它们的绝对补集。
- 相对补集：A与B均是集合，A在B中的相对补集指的是属于A但不属于B的项组成的集合，称作B关于A的相对补集。
- 绝对补集：设S是一个集合，A是S的一个子集，由S中所有不属于A的项组成的集合叫作子集A在S中的绝对补集。

> **相对补集**定义：由属于A而不属于B的元素组成的集合，称为B关于A的相对补集
>
> **绝对补集**定义：A关于全集合U的相对补集称作A的绝对补集

### 高级匹配方法:先行断言



### 基础匹配测试

- 检查邮箱的格式，邮箱格式为`AAAAAAAAA@BBBB.com`
  - 邮箱AAA格式只允许由数字字母下划线组成，第一个字符必须是字母，最后一个字符不得是下划线，长度不得超过9位
  - 邮箱BBB格式为`qq`,`sina`,`foxmail`,`google`,`163`之中任选一个
- 
