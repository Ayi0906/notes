## 需要澄清的一点
- 在js中，通过`new`调用的函数并非与普通函数有什么不同。事实上，包括内置对象函数(比如Number())在内的所有函数都可以用`new`来调用，这种函数调用被称为构造函数调用。
- 也因此，并不存在什么“构造函数”，有的只是对函数的“构造调用”。

## 通过`new`调用函数时会发生以下四项操作
- 创建一个全新的对象
- 新对象会被执行`[[Prototype]]`连接
- 新对象会被绑定到函数调用的`this`
- 如果函数没有返回其它对象，那么就返回所创建的独对象

```
        function a() {
            this.num = 777; // 如果没有这行代码，那么创建的就是一个空对象
            console.log('这是a函数');
        }

        let objA = new a();
        console.log(typeof objA); // object
        for (const prop in objA) {
            console.log(prop); // num
        }
```

> 通过`new`调用函数时，会构造一个新对象并把它绑定到所调用函数的`this`上。