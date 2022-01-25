[Toc]

### 函数参数

```javascript
;(function(w){
    function Swiper(selector,options){
        // 获取最外层结构以及参数
        let oContainer = null
        if (typeof selector === 'object') {
            oContainer = selector
        } else if (typeof selector === 'string') {
            oContainer = document.querySelector(selector)
        }
        let loop = options ? (options.loop || false) : false
        let auto = options ? (options.auto || false) : false
        let switchTime = options ? (options.switchTime || 3000) : 3000
        let pagination = options ? (options.pagination || false) : false
        let callback = options ? (options.callback || false) : false

        // 获取html结构元素及
        let oWrapper = oContainer.querySelector('.swiper-wrapper')
        let aSlide = null
        let oNav = null
        let aLiNav = null
    }
})(window)
```



### 状态变量

```javascript
        // 状态变量以及值变量
        let len = 0 // 如果loop为true，它等于length/2
        let length = 0 // 它等于当前的.slide的数量
        let iNow = 0
        let isHori = true
        let isFirst = true
```

```javascript
```



