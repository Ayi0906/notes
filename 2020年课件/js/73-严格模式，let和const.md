## var\let\const的使用和区别

- var
  - js引擎会预解析var声明的变量，并对var声明的变量进行提升
- let
  - let声明的变量不会预解析，也自然不会进行提升。let声明的变量的作用域是块作用域
- const
  - const定义的是常量，不允许修改，不允许声明不赋值，同样是块级作用域。

## 严格模式与非严格模式的区别

- 1.严格模式不允许使用with。

- 2.严格模式下变量必须声明，赋值给未声明的变量会报错，js引擎不会在严格模式下隐式创建全局变量。

- 3.严格模式的arguments是静态副本，修改arguments不会动态修改实参的值。

- 4.严格模式下禁止使用8进制字面量。

  - ```
        let a = 010
        console.log(a) // Octal literals are not allowed in strict mode
    ```

- 5.严格模式下的eval有自己的词法作用域，其中的声明无法再修改所在作用域的词法作用域

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

- 6.严格模式下this的默认绑定不再自动绑定到widnow对象，而是undefined。显式绑定时如果传入null或undefined，同样采用默认绑定规则，但是多了一个null，绑定至null或者undefined。

- 7.严格模式下删除或修改{configurable:false}的属性时报错，而不是忽略。

- 8.arguments.callee，arguments.caller被禁用