- 水平格式化有七个属性，分别是：`margin-left`,`border-left`,`padding-left`,`width`,`padding-right`,`border-right`,`margin-right`七个。其中只有`margin-left`,`width`,`margin-right`三个属性允许被设置为`auto`。
- 水平格式化七个属性的值加在一起必须等于元素包含块的宽度，这也是允许其中三个属性设置为`auto`的前提。

### 当不设置`auto`值且七个属性值加在一起小于包含块宽度
这种时候用户代理会强制把`margin-right`设置为`auto`（仅限从左到右读的语言）。但是通过`getComputedStyle()`来获取`margin-right`依然得到的是开发者的设定值。

### 当只有一个值设置为auto时
当只有一个值设置为`auto`时，它可以是`width`,`margin-left`,`margin-right`三个中的任一个。只有这个一个未知数时，用户代理会计算得出`auto`的值以保证七个值相加等于元素包含块的宽度。

### 当有两个值设置为auto时
当`margin-left`与`width`设置为`auto`时，用户代理是无法进行计算的，因为同时存在两个未知数。所以用户代理会把`margin-left`属性强制设置为0，从而计算得出`width`属性的值。（同理适用于`margin-right`与`width`属性设置为`auto`的情况）
当`margin-left`与`margin-right`值设置为`auto`时，用户代理会计算出两个值的和，再分别分配给两个值。渲染的结果就是内部元素相对于外部元素水平居中。

### 当有三个值设置为auto时
用户代理是通过计算而不是猜测得值。如果开发者要求用户代理去猜的话，用户代理则会很强硬地将`magin-left`与`margin-right`强制设置为0，计算得出`width`的值。
