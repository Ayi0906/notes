[Toc]

## 创建script元素对象，赋值src属性来调用指定的脚本

```html
<body>
    <!-- 动态脚本 -->
    <script>
        let oScript = document.createElement("script")
        oScript.src = './test8.js'
        document.body.appendChild(oScript)
    </script>
</body>
```

### 封装loadScript函数

- 通过传入参数src来在文档里调用指定的脚本

```javascript
        function loadScript(src) {
            let oScript = document.createElement('script')
            oScript.src = './' + src
            document.body.appendChild(oScript)
        }
        loadScript('test8.js')
```

## 通过嵌入源代码的方式动态加载脚本

```javascript
        let oScript = document.createElement('script')
        oScript.appendChild(document.createTextNode('alert(\'xxx\')'))
        document.body.appendChild(oScript)
```

- IE10需要将let修改为var
- IE8以及以下不支持运行这段代码
- innerText与innerHTML也同样可以代替`document.createTextNode()`

### 旧版IE兼容性嵌入源代码

- 通过.text属性添加代码，适配IE8以及以下的浏览器

- ```javascript
          var oScript = document.createElement('script')
          oScript.text = 'alert(\'xxx\')'
          document.body.appendChild(oScript)
  ```

## 跨浏览器函数

```javascript
        function loadScriptString(codeStr) {
            var oScript = document.createElement('script')
            oScript.type = 'text/javascript'
            try {
                oScript.appendChild(document.createTextNode(codeStr))
            } catch (err) {
                oScript.text = codeStr
            }
            document.body.appendChild(oScript)
        }

        var codeStr = 'alert(\'xxx\')'
        loadScriptString(codeStr)
```

## 通过innerHTML属性创建的`<script>`永不执行

- 需要区分这里指的是通过innerHtml创建`<script>`元素

  - ```javascript
        <script>
            document.body.innerHTML = '<script>alert(\'xxx\')<\/script>'
        </script>
    ```

  - 浏览器会创建`<script>`元素以及文本，但是js引擎会给其打上“永不执行”的标签，js引擎不会执行这些代码也不允许通过任何方式强制执行。