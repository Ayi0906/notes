- `$('#app')` 代替 `document.getElementById('app)`
- `$('p')` 代替 `document.getElementsByTagName('p')`


## 区别
- 原生获取不存在的DOM对象时会报错，但jQuery不会

### 检查某个元素是否存在
- 检查jQuery获取的伪数组对象的长度
```
if($('#app').length>0){}
```
- 转换为DOM对象
```
if($('#app')[0]){}
```

## 基本选择器（5个）
- id: `$('#app');`
- 类: `$('.app');`
- 元素: `$('p');`
- all: `$('*');`
- 集合: `$('#app1','#app2')`

## 层次选择器（4个）
- 后代选择器: `$('div span');`选择div下所有span元素，不管嵌套了多少层
- 子代选择器: `$('div>span');`选择div子代里的所有span元素
- next选择器: `$('div+span');`选择div元素后面紧接着的那个span元素，如果div后面的元素不是span，则获取失败
  - `$('div').next('span');`
- 队列选择器: `$('div~span');`选择div元素后面所有的span元素。
  - `$('div').nextAll('span');`

## 过滤选择器
### 基本过滤选择器（11个）
- 首尾2，否定1，奇偶2，索引3，header1，动画1，焦点1
- `$('div:first');`
- `$('div:last');`
- `$('div:not(.d1)');` 选取div元素，但不包含类名为d1的元素
- `$('div:even');` 偶数，索引从0开始
- `$('div:odd');` 奇数：索引从1开始
- `$('input:eq(1)');` 选取索引等于1的元素，这个索引从0开始计算
- `$('input:gt(1)');` 选取索引大于1的元素，不等于1
- `$('input:lt(2)');` 选取索引小于2的元素，不等于2
- `$(':header');` 选取所有h1,h2,h3...元素
- `$('div:animated');` 选取正在执行动画的元素
- `$(':focus');` 选取当前获取焦点的元素

### 内容过滤选择器（4个）
- `$('div:contains('我')');` 选择含有文本“我”的div元素，注意与has的区别它只能筛选文本
- `$('div:empty');`  选择不包含子元素的div元素
- `$('div:has(p)');` 选择含有p元素的div元素
- `$('div:parent');` 选择含有子元素或文本的div元素

### 可见性过滤选择器（2个）
- `$(':hidden')` 选择所有不可见元素。包括`<input type="hidden">`,`<div style="display:none">`,`<div style="visibility:hidden">`三种，如果想要选择input则`$('input:hidden');`
- `$(':visible')` 选取所有可见元素

### 属性过滤选择器（9个）
==下面的这些用于匹配的字符串是可以加引号的==
- `$('div[id]')` 选取有id属性的div元素
- `$('div[id=test]')` 选取id属性为test的元素
- `$('div[id!=test]')` 选取id属性不为test的元素
- `$('div[id^=color]')` 选取id属性以color为开头的元素
- `$('div[id$=red]')` 选取id属性以red为结尾的元素
- `$('div[id*=test]')` 选取id属性包含有test字样的元素
- `$('div[id|=en]')` 选取id属性为'en'或者id属性以'en-'为开头的元素
- `$('div[class~="top"]')` 选取class属性中用空格分隔的值中包含有 top 字样的div元素
- `$('div[id][title^="test"]')` 选取有id属性，且title以test为开头的div元素

### 子元素过滤选择器（4个）
- `$('nth-child(index/even/odd/equation)')`
  - `$('div :nth-child(1)')`  nth-child的index从1开始算起，选择div下的第一个子代元素（==注意空格==）
  - `$('div p:nth-child(1)')`  选择div子代下第一个p元素，如果div子代第一个元素不是p，则无法获取
  - `$('div p:nth-child(2n)')` 选择div子代下第2，4，6，8个p元素
- `$('ul>li:first-child')` 选取ul子代li元素中的第一个元素
- `$('ul>li:last-child')` 选取ul子代li元素中最后一个元素
- `$('ul>li:only-child')` 如果li元素是ul元素子代中唯一的元素，那么则予以匹配

### 表单对象属性过滤选择器（4个）
- `$('#from1 :enabled');` 选取id为from1的表单内的所有可用元素
- `$('#form2 :disabled')` 选取id为from2的表单内的所有不可用元素
- `$('input:checked')` 选取所有被选中的`<input>`元素
- `$('select option:selected')` 选取所有被选中的选项元素

## 表单选择器（11个）
- `$(':input')` 选择所有`<input>`,`<textarea>`,`<select>`,`<button>`元素
- `$(':text')` 选择所有单行文本框
- `$(':password')` 选择所有密码框
- /radio/checkbox/submit/image/reset/button/file/hidden

## 一些版本遗留问题
- 在jQuery1.3.1以下版本中属性选择器前需要加@符号，但1.3.1版本以上废弃掉了这个符号
  - `$('div[@title=test]')`