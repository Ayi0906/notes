## 版本

- 1.x：兼容老版本IE，文件更大。使用率最高。
- 2.x：部分IE8及以下不支持
- 3.x：完全不支持IE8以下，提供新API，提供不包括ajax/动画API的版本

## 工具方法

### `$.each()`

- 遍历对象中的数据

### `$.trim`

- 去除字符串两边的空格

### `$.type(obj)`

- 得到数据的类型

### `$.iSArray(obj)`

- 判断是否是数组

### `$.isFunction(obj)`

- 判断是否是函数

### `$.parseJSON(json)`

- 解析json字符串转换为js对象/数组

## 操作属性

1. 读取第一个div的title属性
   1. 考察.attr()

1. 给所有的div设置name属性
   1. 移除所有的div的title属性
2. 给所有的div设置class="guiguClass"
3. 给所有的div添加class="abc"
4. 移除所有div的guiguClass的class
5. 得到最后一个li的标签体文本
6. 设置第一个li的标签体为"<h1>mmmmmmmm</h1>"
7. 得到输入框中的value值
8. 将输入框的值设置为atguigu
9. 点击“全选按钮”实现全选
10. 点击”全不选“按钮实现全不选

## css方法

- 得到第一个p标签的颜色
- 设置所有p标签的文本颜色为pink
- 设置第二个p的字体颜色(#ff0011)，背景(blue)，宽(300px)，高(30px)

## offset和position

- offset()相对于页面左上角的坐标
- position()相对于父元素左上角的坐标（只对可见元素有效）

## scrollTop

- `scrollTop()`:读取/设置滚动条的y坐标
- `$(document.body).scrollTop()+$(document.documentElement).scrollTop()`：读取页面滚动条的y坐标（兼容Chrome和IE）
- `$('body,html').scrollTop(60)`：滚动到指定位置（兼容chrome和IE）

## 元素的尺寸

- 内容尺寸
  - height()：height
  - width()：width
- 内部尺寸
  - innerHeight()：height+padding
  - innerWidth()：width+padding
- 外部尺寸
  - outerHeight(false/true)：height+padding+border，如果是true，加上margin
  - outerWidth(false/true)：width+padding+border，如果是true，加上margin

## 筛选过滤

1. ul下li标签第一个
2. ul下li标签最后一个
3. ul下li标签第二个
4. ul下li标签中title属性为hello
5. ul下li标签中title属性不为hello的
6. ul下li标签中有span子标签的
   1. 考察.has()的用法

## 回到顶部

- 单位时间内回到顶部

## 父母-孩子-兄弟

1. children()
2. find()
3. parent()
4. prevAll()
5. nextAll()
6. siblings()



1. ul标签的第二个span子标签
2. ul标签的第2个span后代标签
3. ul标签的父标签
4. id为cc的li标签的前面的所有li标签
5. id为cc的li标签的所有兄弟li标签

## 文档增删改查

1. append(content)
2. perpend(content)
3. before(content)
4. after(content)
5. replaceWith(content)
6. empty()
7. remove()



1. 向id为ul1的ul下添加一个span（最后）
2. 向id为ul1的ul下添加一个span（最前）
3. 在id为ul1的ul下的li（title为hello）的前面添加span
4. 在id为ul1的ul下的li（titl为hello）的后面添加span
5. 在id为ul2的ul下的li（title为hello）全部替换为p
6. 移除id为ul2的ul下的所有li

## 添加删除记录

## 爱好选择器

- 点击“全选”：选中所有爱好
- 点击“全不选”：所有爱好都不勾选
- 点击“反选”：改变所有爱好的勾选状态
- 点击“全选/全不选”：选中所有爱好或者全不选中
- 点击某个爱好时，必要时更新“全选/全不选”的选中状态
- 点击“提交”：提示所有勾选的爱好

## 事件绑定与解绑

\1. 事件绑定(2种)：

 \* eventName(function(){})

  绑定对应事件名的监听, 例如：$('#div').click(function(){});

 \* on(eventName, funcion(){})

  通用的绑定事件监听, 例如：$('#div').on('click', function(){})

 \* 优缺点:

  eventName: 编码方便, 但只能加一个监听, 且有的事件监听不支持

  on: 编码不方便, 可以添加多个监听, 且更通用

\2. 事件解绑：

 \* off(eventName)

*需求：*

  *1. 给.out绑定点击监听(用两种方法绑定)*

  *2. 给.inner绑定鼠标移入和移出的事件监听(用3种方法绑定)*

  *3. 点击btn1解除.inner上的所有事件监听*

  *4. 点击btn2解除.inner上的mouseover事件*



## 事件委托

*1. 事件委托:*

 ** 将多个子元素(li)的事件监听委托给父辈元素(ul)处理*

 ** 监听回调是加在了父辈元素上*

 ** 当操作任何一个子元素(li)时, 事件会冒泡到父辈元素(ul)*

 ** 父辈元素不会直接处理事件, 而是根据event.target得到发生事件的子元素(li), 通过这个子元素调用事件回调函数*

*2. 事件委托的2方:*

 ** 委托方: 业主 li*

 ** 被委托方: 中介 ul*

*3. 使用事件委托的好处*

 ** 添加新的子元素, 自动有事件响应处理*

 ** 减少事件监听的数量: n==>1*

*4. jQuery的事件委托API*

 ** 设置事件委托: $(parentSelector).delegate(childrenSelector, eventName, callback)*

 ** 移除事件委托: $(parentSelector).undelegate(eventName)*

*事件委托 给父元素绑定delegate 第一个参数 为需要代理的子元素  第二个参数 事件名称 第三个参数 回调函数*

## 动画方法

### 淡入淡出

*淡入淡出: 不断改变元素的透明度来实现的*

*1. fadeIn(): 带动画的显示*

*2. fadeOut(): 带动画隐藏*

*3. fadeToggle(): 带动画切换显示/隐藏*

  *需求：*

  *1. 点击btn1, 慢慢淡出*

   ** 无参*

   ** 有参*

​    ** 字符串参数*

​    ** 数字参数*

  *2. 点击btn3, 慢慢淡入*

  *3. 点击btn3, 淡出/淡入切换，动画结束时提示“动画结束了”*

### 滑动

*滑动动画*

*1. slideDown(): 带动画的展开*

*2. slideUp(): 带动画的收缩*

*3. slideToggle(): 带动画的切换展开/收缩*

  *需求：*

  *1. 点击btn1, 向上滑动*

  *2. 点击btn3, 向下滑动*

  *3. 点击btn3, 向上/向下切换*

### 显示与隐藏

*显示隐藏，默认没有动画*

*1. show(): (不)带动画的显示*

*2. hide(): (不)带动画的隐藏*

*3. toggle(): (不)带动画的切换显示/隐藏*

 *需求:*

 *1. 点击btn1, 立即显示*

 *2. 点击btn2, 慢慢显示*

 *3. 点击btn3, 慢慢隐藏*

 *4. 点击btn4, 切换显示/隐藏*

## 自定义动画

*jQuery动画本质 : 在指定时间内不断改变元素样式值来实现的*

*1. animate(): 自定义动画效果的动画*

*2. stop(): 停止动画*

  *需求：*

  *1.逐渐扩大*

   *1.宽度扩大*

   *2.宽度和高度扩大*

   *3.先宽度扩大后高度扩大*

  *2.向右移动*

   *1.向右移动 固定的200px*

   *2.基于当前位置向右移动 200px*

  *3.向左移动*

   *1.向右移动 固定的200px*

   *2.基于当前位置向右移动 200px*

   *3.判断红框是否正在移动，如果正在移动的时候点击‘向左’按钮，打印出：‘别闹，正在动呢...’*

  *4.停止动画*

   *1.停止所有的动画*

   *2.停止当前动画，直接开始下一个动画*

   *3.停止并结束当前动画，开始下一个动画*

## 基础轮播翻页

## 无限滚动

