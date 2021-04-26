- 箭头函数不适用于四种绑定规则。它是根据箭头函数声明的地方决定它的`this`的。
- 箭头函数会继承其声明所在的词法作用域的`this`值。

### 证明箭头函数的`this`值继承于它所在的词法作用域
```
        function foo() {
            return () => {
                console.log(this.num);
            }
        }

        let obj1 = {
            num: 7
        };
        let obj2 = {
            num: 9
        };

        let getNum = foo.call(obj1);
        getNum.call(obj2); // 7
```
- `getNum`接收了返回的箭头函数。此时函数`foo`是通过显示绑定将`this`绑定到对象`obj1`上。
- 将返回的箭头函数再通过显式绑定绑定至`obj2`上。返回的`this.a`的值却是`obj1.a`的值。说明箭头函数的`this`值继承至其所在的词法作用域的this值，且不可修改。

### 箭头函数常用于回调函数，分析回调函数的`this`值
```
        function foo() {
            let timer = setTimeout(() => {
                console.log(this);
            }, 1000);
        }

        let obj = {
            foo: foo
        }

        foo(); //  window
        obj.foo(); // object
```

### 箭头函数实现的一种ES5中的模式
```
        function foo() {
            let that = this;
            setTimeout(function () {
                console.log(that.num);
            }, 100);
        }

        let obj = {
            num: 7
        };
        foo.call(obj); // 7
```