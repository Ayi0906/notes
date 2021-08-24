[Toc]

## innerText与TextContent

- 作用几乎相同。区别在于textContent可以获取隐藏元素的文本内容以及内容中的换行符

- innerText：任何浏览器

- textContent：>=IE9

- 兼容性案例：

  - ```
            function setOrGetContent(obj, value) {
                if (arguments.length === 2) {
                    // 设置obj的文本
                    if (obj.textContent) {
                        obj.textContent = value
                        console.log('textContent 可用')
                    } else {
                        console.log('innerText 可用 textContent不可用')
                        obj.innerText = value
          
                    }
                } else {
                    var res = obj.textContent ? obj.textContent : obj.innerText
                    return res
                }
            }
    ```

    

## firstChild与firstElementChild

- firstChild会获取第一个子节点，但第一个子节点通常是文本节点（标签之间的空白）

- firstElementChild仅适用于高级浏览器

- 通过检测nodeType来获取元素的第一个子元素

- ```
          function getFirstEle(obj) {
              if (obj.firstElementChild) {
                  return obj.firstElementChild
              } else {
                  var testNode = obj.firstChild
                  while (testNode && testNode.nodeType !== 1) {
                      testNode = testNode.nextSibling
                  }
                  return testNode
              }
          }
  ```

## 跨浏览器的事件处理程序

- DOM2级事件处理程序`addEventListener`只兼容>=IE9
- DOM0级事件处理程序(onclick)可以适用于任何浏览器
- IE事件处理程序`attachEvent`

```
        var EventUtil = {
            addHandler: function (element, type, handler) {
                if (element.addEventListener) {
                    element.addEventListener(type, handler, false)
                } else if (element.attachEvent) {
                    element.attachEvent('on' + type, handler)
                } else {
                    element['on' + type] = handler
                }
            },
            removeHandler = function (element, type, handler) {
                if (element.removeEventListener) {
                    element.removeEventListener(type, handler, false)
                } else if (element.detachEvent) {
                    element.detachEvent('on' + type, handler)
                } else {
                    element. ['on' + type] = null
                }
            }
        }
```



