- javascript如何判断一个Number类型变量为整数

  - 通过%1取余判断,整数取余为0，小数取余为小数

    - ```javascript
              let num=7.5
              console.log(num%1) // 0.5
      ```

  - 通过Math.floor取整判断

    - 整数取整后还是自己

    - ```javascript
      let num=7.5
      Math.floor(num)===num // false
      ```

  - 通过parseInt判断，原理同上

    - ```javascript
              let num = 77.5
              console.log(parseInt(num, 10) === num) //false
      ```

  - 通过位运算符判断

    - ```javascript
              function isInteger(num) {
                  return (num | 0) === num
              }
              console.log(isInteger(7.5)) // fasle
              console.log(isInteger(7)) // true
      ```

  - ES6提供了`Number.isInteger`

    - ```javascript
              console.log(Number.isInteger(7.5)) // false
              console.log(Number.isInteger(7)) // true
      ```



[五种js判断是否为整数类型方式](https://www.cnblogs.com/yueguanguanyun/p/7255962.html)