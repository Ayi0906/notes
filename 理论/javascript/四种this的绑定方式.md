[Toc]



## 默认绑定

- 通过`fn()`这种方式调用的就是默认绑定。默认绑定在非严格模式下绑定到window对象上。

## 隐式绑定

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

## 显式绑定

- 通过`fn.call(obj)`就是显式绑定，除此之外还有apply，bind
  - call与apply的区别是call的参数是通过逗号间隔的字符串，apply是数组
- 将null或者undefined作为this的绑定对象传入call，apply或者bind，这些值在调用时会被忽略掉。应用默认绑定规则。

## new绑定

- 通过`obj = new fn()`方式调用。this绑定在obj上。

