[Toc]

## warning: Unexpected unnamed function (func-names)
- 看到这个提示基本是就是说你的函数不能是匿名函数，最好可以起一个名字，然后你增加一个函数名称就好了
- 报错代码：
```
window.onresize=function(){
    // some code
}
```
改为：
```
function fnResize(){
    // some code
}
window.onresize=fnResize;
```

## Comments are not permitted in JSON
- 将右下角的配置 JSON 更改为 JSON with Comments 即可

