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

## 插入节点（8个）
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

## 删除节点（3个）
- `remove()`:删除指定的元素，返回值是被删除的元素，可以将它重新插入到其它位置。
  - ex.删除ul#ul1节点中第2个li元素节点，并将它插入到ul#ul2节点
  ```
  <ul id="ul1">
    <li title="a">aaa</li>
    <li title="b">bbb</li>
  </ul>
  <ul id="ul2"></ul>

  let $li = $('#ul1>li:eq(1)').remove();
  $('#ul2').append($li);
  ```
  - ex.使用appendTo简化以上代码
  ```
  $('#ul1>li:eq(1)').appendTo($('#ul2'));
  ```
  - ex.给remove()传递参数选择性删除title属性为a的li元素
  ```
  $('#ul1>li').remove('li[title=a]');
  ```
- `detach()`:与remove功能相同，删除某一个指定的元素，不同点在于detach会将绑定的事件，附加的数据保留下来
  - ex.删除元素，再添加，测试绑定的事件监听依然存在；但是使用remove方法则不存在
  ```
  $('#ul1>li:eq(1)').click(function () {
            alert($(this).html());
        });
  var $li = $('#ul1>li:eq(1)').detach();
  $li.appendTo($('#ul2')); // 点击弹出 'bbb'字符串
  ```
- `empty()`:用以清空元素节点。包括元素的文本节点和元素的子代元素。
  - ex:清空指定的li元素的文本，清空指定的ul元素的后代元素
  ```
  $('#ul1>li:eq(0)').empty(); // 清空li文本
  $('#ul1').empty(); // 清空ul子代元素
  ```
## 复制节点（1个）
- clone
  - ex.单击复制一个<li>元素到#ul2中
  ```
        $('#ul1>li').click(function () {
            $(this).clone().appendTo($('#ul2'));
        })
  ```
  - ex.复制元素的功能-->给clone加入参数true
  ```
        <button id="btn">click</button>

          $('#btn').click(function () {
            $(this).clone(true).appendTo('body');
        });
  ```
## 替换节点（2个）
- replaceWith()
  - ex.将某一个节点替换为另一个节点
  ```
  $('#ul1>li:eq(1)').replaceWith('<li title="c">ccc</li>');
  ``` 
- replaceAll():replaceWith的倒装用法
```
$('<li title="d">ddd</li>').replaceAll($('#ul1>li:eq(1)'));
```

## 包裹节点（3个）
- `wrap()`:为匹配的元素添加一个包裹元素
```
<strong>some text</strong>
<strong>other some text</strong>

$('strong').wrap('<b></b>');
// <b><strong>other some text</strong></b>
// <b><strong>other some text</strong></b>
```
- `wrapAll()`:为匹配的所有元素用一个元素来包裹
```
$('strong').wrapAll('<b></b>');

// <b>
//  <strong>some text</strong>
//  <strong>other some text</strong>
// </b>
```
- `wrapInner()`:将每一个匹配的元素的子内容（包括文本节点）用其它结构化的标记包裹起来
```
$('strong').wrapInner('<b></b>');

// <strong><b>some text</b></strong>
// <strong><b>other some text</b></strong>
```

## 属性操作(2个)
- 使用`attr()`方法来获取和设置元素属性，`removeAttr()`方法来删除元素属性
- 获取元素属性
```
let $li_title = $('#ul1>li:eq(0)').attr('title');
console.log($li_title); // a
```

- 设置元素属性
```
$('#ul1>li:eq(0)').attr('title','aa'); // 设置单个属性
        $('#ul1>li:eq(0)').attr({
            'title': 'aa',
            id: 'li1'
        }); // 设置多个属性
```
- 删除属性
```
$('#ul1>li:eq(0)').removeAttr('title');
```

## 样式操作(4个)
- 获取样式
  -  为attr方法传入参数
```
$('#ul1>li:eq(0)').attr('title');
```
- 替换样式
  - 为attr方法闯入两个参数，需要注意的是此时是替换而不是追加
```
<div id="d1" class="box1"></div>

$('#d1').attr('class','box2'); // class被替换为了box2
```
- 追加样式
  - addClass,当样式冲突时，根据样式表选择器优先级决定
```
$('#d1').addClass('box2'); // class值为'box1 box2'
```

- 移除样式
  - `removeClass()`:删除指定样式则传入一个参数，删除多个样式传入参数以空格隔开，删除全部样式则不传入参数
- 切换样式
  - `toggleClass()`:如果类名存在则删除它，如果类名不存在则添加它
```
<div id="d1" class="box1"></div>

$('#btn').click(function(){
    $('#d1').toggleClass('box1');
})
// 点击按钮，div的类名会在'box1'与' '之间进行切换 
```
```
$('#btn').click(function(){
    $('#d1').toggleClass('box2');
})
// 点击按钮，div的类名会在'box1'与'box1 box2'之间切换
```
- 判断样式存在与否
  - hasClass:判断元素是否有某个元素，有返回true，没有返回false

## 设置和获取HTML、文本和值(3个)
- `html()`:类似`innerHTML`方法，读取或设置某个元素的HTMLe内容
  - 获取：
```
alert($('#btn').html());
// click
alert($('ul').html());
// <li title="a">aaa</li>
// <li title="b">bbb</li>
$('strong:eq(0)').html('<span style="color:red;">Hello</span>');
//<strong><span style="color:red;">Hello</span></strong>
```
- `text()`:类似`innerText`方法，用来获取或设置文本内容
```
$('strong:eq(0)').text('Hello');
// <strong>Hello</strong>
```
- `val()`:类似`value()`方法，设置或获取元素的值
  - 获取值与设置值得方式与`html()`和`tesxt()`一致
  - 有些时候需要将值清空:`$( //some code ).val('');`
  - 下拉框、多选框、单选框的选定
```
<select name="" id="s1" multiple style="height: 50px;">
    <option>text1</option>
    <option>text2</option>
    <option>text3</option>
</select>
<input type="radio" id="r1" value="radio1">radio1
<input type="radio" id="r1" value="radio2">radio2
<input type="radio" id="r1" value="radio3">radio3
<input type="checkbox" name="fruit" value="orange">orange
<input type="checkbox" name="fruit" value="banana">banana
<input type="checkbox" name="fruit" value="strawberry">strawberry

$('#s1').val(["text2", "text3"]);
$(':radio').val(["radio2"]);
$(':checkbox').val(["orange", "strawberry"]);
// $().attr('checked',true) 也可以达到同样的效果
```

## 遍历节点(5个)
- `children()`方法：获取元素的子元素，仅获取子元素;它接收jquery选择器用以筛选子元素。
```
$('ul').children().length; // 2
$('ul').children('li[title=a]').length; // 1
```
- `next()`方法：匹配元素后面紧邻的同辈元素
- `prev()`方法：匹配元素前面紧邻的同辈元素
- `sibling()`方法：匹配元素前后所有的同辈元素，不会匹配到它自己
```
$('#ul1>li:eq(0)').siblings().attr('title'); // b
```
- `cloest()`方法：从当前元素开始，返回被选元素的第一个祖先元素。祖先是父、祖父、曾祖父，依此类推。
```
$('#ul1>li:eq(0)').closest('li')[0]; // <li title="a">aaa</li>
$('#ul1>li:eq(0)').closest('ul')[0]; // <ul id="ul1"></ul>
```
- parent\parents\closest的区别
  -  parent获取距离选取的元素最近的父元素。
  -  parents获取所选取元素的所有祖先元素，传入参数可以指定选取一个类型的祖先元素
  -  closest获取距离选取元素最近的祖先元素，从它自己本身开始算起，同时接收参数用以筛选。

## CSS-DOM操作
- `css()`方法
  - 直接利用`css()`方法获取元素的样式属性
    - `$('p').css('color');`
  - 设置元素的属性
    - `$('p').css({"fontSize":"30px","backgroundColor":"red"});`
- `height()`方法
  - 获取元素的高度
    - `$('p').height();`
    - 与`$(element).css("height")`的区别在于，`height()`方法获取的是计算值，以px为单位。`css()`方法获取的是样式表的属性值，可能是auto之类的值。 
  - 设置元素的高度
    - `$('p').height(100)`:设置p元素的高度为100px
    - `$('p').height("10em")`:设置p元素高度为10em
- `width()`方法
  - 与`height()`方法一致
- `offset()`方法
- `position()`方法
- `scrollTop()`方法与`scrollLeft()`方法 