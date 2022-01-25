[Toc]

## &&符号

```javascript
        let a = true && 5
        console.log(a) // 5
        let b = false && 6
        console.log(b) // false
        let c = 1 && 7
        console.log(c) // 7
        let d = 0 && 8
        console.log(d) // 0
```

- 如上所示，以`statement1 && statement2`为例
  - `statement1`的布尔值为true时，返回`statement2`的值
  - `statement1`的布尔值为false时，返回`statement1`的值



### &&符号并不是简单的return一个值

```javascript
        function fn() {
            let a = 1 && console.log('aaaaaa')
            console.log('bbbbbb')
        }

        fn() // 'aaaaaa'  // 'bbbbbb'
```

- 可以看到执行函数第一条语句后，第二条语句依然执行。因此&&并不是简单的作判断后return一个值。它更类似于IIFE。

```javascript
        function fn() {
            let a = (function () {
                if (1) {
                    return console.log('aaaaaa')
                }
            })()
            console.log('bbbbbb')
        }
        fn() // 'aaaaaa'  // 'bbbbbb'
```

## ||符号

```javascript
        let a = true || 5
        console.log(a) // true
        let b = false || 6
        console.log(b) // 6
```

- 如上所示，以代码`statement1 ||statement2`为例
  - 如果`statement1`的布尔值为true，那么返回`statement1`的值
  - 如果`statement1`的布尔值为false，那么返回`statement2`的值

## 多个&&符号连接的语句

```javascript
true&&console.log('aaaa')&&console.log('bbbb') // 'aaaa'
```

```javascript
        function fn(){
            console.log('aaaa')
            return true
        }
        true&&fn()&&console.log('bbbb') // 'aaaa' 'bbbb'
```



- 以`statement1 && statement2 && statement3`为例：
  - 当`statement1`的布尔值为true时，`statement2`被返回（如果`statement2`是一条语句的话，它会被执行）
  - 对`statement2`返回值的布尔值进行判断，为true时返回`statement3`，为false时结束语句

## 多个||符号连接的语句

```javascript
true||console.log('aaaa')||console.log('bbbb')  // 返回true值，不执行后面的语句
false||console.log('cccc')||console.log('dddd') // 'cccc' 'dddd'
```

```javascript
        function fn1() {
            console.log('aaa')
        }

        function fn2() {
            console.log('bbb')
        }

        function fn3() {
            console.log('ccc')
        }
        fn1()||fn2()||fn3() // 'aaa' 'bbb' 'ccc'
```



- 以`statement1||statement2||statement3`为例：
  - 当`statement1`的布尔值为false时，才能返回后面的值或语句,对`statement2`语句进行执行，并判断它的返回值
    - 如果`statement2`语句返回值的布尔值为true，那么停止执行剩下的语句
    - 如果`statement2`语句返回值的布尔值为false，那么继续执行下一条语句
  - 当`statement1`的布尔值为true时，返回true，并停止执行判断后面的语句