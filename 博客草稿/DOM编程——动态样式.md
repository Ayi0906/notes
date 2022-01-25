[Toc]

## 通过新创建`<link>`元素，赋值其`src`属性来达到动态插入样式的目的

```javascript
        let oLink = document.createElement('link')
        oLink.rel = 'stylesheet'
        oLink.type = 'text/css'
        oLink.href = url
        let oHead = document.head // html5新增的属性，指向文档<head>元素
        oHead.appendChild(oLink)
```

### 封装以上代码

```javascript
        function loadStyles(url) {
            let oLink = document.createElement('link')
            oLink.rel = 'stylesheet'
            oLink.type = 'text/css'
            oLink.href = url
            let oHead = document.head
            oHead.appendChild(oLink)
        }
```

## 通过设置样式文本为`<style>`元素创建样式

```javascript
        let oStyle = document.createElement('style')
        oStyle.type = "text/css"
        oStyle.appendChild(document.createTextNode("div{width:200px;height:200px;background-color:#456;}"))
        let oHead = document.head
        oHead.appendChild(oStyle)
```

- 上面的代码在IE8以及以下版本IE中是无法运行的。另外IE10运行的话需要修改let为var。

### 解决IE创建动态样式失败

- IE8以及以下元素需要访问styleSheet属性，再通过这个属性下的cssText属性来添加css代码

- ```javascript
          var oStyle = document.createElement('style')
          oStyle.type = "text/css"
          try {
              oStyle.appendChild(document.createTextNode("div{width:200px;height:200px;background-color:#456;}"))
          } catch (ex) {
              oStyle.styleSheet.cssText = "div{width:200px;height:200px;background-color:#456;}"
          }
          var oHead = document.getElementsByTagName('head')[0]  // IE8不支持document.head
          oHead.appendChild(oStyle)
  ```

#### 封装一个跨浏览器的样式写入函数

```javascript
        function loadStyleString(styleStr) {
            var oStyle = document.createElement('style')
            oStyle.type = "text/css"
            var oHead = null
            try {
                oHead = document.head
                oStyle.appendChild(document.createTextNode(styleStr))
            } catch (ex) {
                oHead = document.getElementsByTagName('head')[0]
                oStyle.styleSheet.cssText = styleStr
            }
            oHead.appendChild(oStyle)
        }

        var cssText = 'div{width:200px;height:200px;background-color:#789;}'
        loadStyleString(cssText)
```

