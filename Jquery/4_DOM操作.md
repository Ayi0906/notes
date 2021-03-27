- DOM操作分为DOM核心、HTML-DOM、CSS-DOM

# jQuery中的DOM操作
## 查找节点
### 查找元素节点
- ex.查找ul元素下第2个li元素
`const $li=$('ul li:eq(1)');`

### 查找属性节点
- ex.获取p元素的title属性
`const p_txt=$('p').attr("title")`

## 创建节点
### 创建元素节点
- ex.在ul元素下创建两个li元素
```
        var $li_1 = $('<li>');
        var $li_2 = $('<li>');
        $('ul').append($li_1);
        $('ul').append($li_2);
```

### 创建文本节点
- 在创建元素节点时一起创建
```
        var $li_1 = $('<li>香蕉</li>');
        var $li_2 = $('<li>苹果</li>');
        $('ul').append($li_1);
        $('ul').append($li_2);
```

### 创建属性节点
- 在创建元素节点时一起创建
```
        var $li_1 = $('<li title="orange">orange</li>');
        var $li_2 = $('<li title="apple">apple</li>');
        $('ul').append($li_1);
        $('ul').append($li_2);
```

## 插入节点
- `append()`:向匹配元素内部追加内容
  - ex1:将li元素添加到ul元素下
    ```
    $('ul').append('<li>list1</li>')
    ```
  - ex2:将b元素及其文本添加到p元素中
    ```
    <p>hello </p>

    $('p').append('<b>jquery</b>'); // <p>hello <b>jquery</b></p>
    ```
- `appendTo()`:append的倒装用法
  - ex1:将b元素及其文本添加到p元素中
    ```
    <p>hello</p>

    $('<b>jquery</b>').appendTo('p');
     ```
- `prepend()`:向每个匹配的元素内部前置内容，注意是在元素内部，不是在元素前面
  - ex1
    ```
    <span>dayo</span><br>
    <span>dayo</span><br>
    <span>dayo</span>

    $('span').prepend('<i>koko </i>');
    // <span><i>koko </i>dayo</span>
    ```
- `prependTo()`:prepend的倒装用法
- `after()`:向每个匹配的元素之后插入内容，注意是元素之后
- `insertAfter()`:after方法的倒装用法
- `before()`:向匹配元素前面插入内容，注意是元素之前
- `insertBefore()`:before的倒装用法

## 删除节点
