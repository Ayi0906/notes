## 在其它库之前使用
### 放弃 $ 符号，使用jQuery
```
jQuery.noConflict(); // 放弃jQuery符号
```

### 使用其它符号代替$符号
```
const $j=jQuery.noConflict();
```

### 形参内传入$
```
jQuery.noConfilct();
jQuery(($)=>{
    var cr=$('#app');
})
```