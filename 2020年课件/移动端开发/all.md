[Toc]

## 移动端特点

- 小屏幕，触摸交互，屏幕尺寸繁多

## 屏幕大小

- 指屏幕的对角线长度，单位一般是英寸（inch）。常见的是3.5（iphone4）,4,4.7（iphone6），5.0,5.5,6.0（iphone6 plus）

- i inch = 2.54 cm

## 屏幕分辨率

- 屏幕横纵向上的像素点数，一般表示形式是`x*y`或者`y*x`表示，例如华为P30分辨率是`2340*1080`:水平方向2340的像素点，竖直方向上有1080个像素点
- 屏幕分辨率是固定值，出厂就有的，不可更改。

## 显示分辨率

- 计算机可以修改显示分辨率，由CPU与屏幕共同决定。
- 1080P的分辨率是1920X1080
- 720P是1280X720
- 2K屏幕是单一方向分辨率具有约2000像素的显示设备。最标准的2K分辨率是2048X1024

## 物理像素 / 设备像素

- 1个物理像素对应显示设备中一个微小的物理部件。(可以看做变色彩灯，由三个分别发红绿蓝色的二极管封装而成，通过控制每一支二极管的电流来混合出不同颜色的光)。
- 物理像素代表屏幕上有多少个像素点（彩灯），比如1920X1080上有2073600个像素点。这个是硬件设备，由制造商决定。
- 物理像素一般并不规则，不同颜色发光二极管的寿命并不同，蓝色二极管寿命最短。所以就有了钻石排列等排列方式。
- 不同厂家制作的屏幕设备是不同的，也没有硬性规定一个物理像素的长宽，像素点之间的间隔。因此如果按照物理像素来进行显示，可能在一个屏幕上测量长10cm的元素，在另一个屏幕上测量长度就变成了15cm。也因此需要设备独立像素来进行重新计算，保证每一个屏幕上的元素显示的大小是相同的。

## 设备独立像素（DIP，安卓中称为DP）

- 基于计算机控制的坐标系统和抽象像素（虚拟像素）
- 为了保证不同用户界面看到的元素一样大，需要保证 设备独立像素
- 设备独立像素由制造商决定。例如iphone6 的设备独立像素为 375 * 667
- 一个设备独立像素代表计算机可以控制的坐标系统中的一个点。
  - 例如iphone6的坐标轴是375X667，但其物理像素是750X1334。也就是说在iphone6里，一个设备独立像由四个物理像素来进行显示。
  - 例如iphone12ProMax物理像素1284X2778，设备独立像素428X926，也就是说在iphone12 Pro Max中一个设备独立像素由9个物理像素来进行显示
  - 普通屏幕下一个设备独立像素控制一个物理像素（封装二极管）
  - 高清屏幕下一个设备独立像素控制N个物理像素 

## Retina屏幕

- 高清屏幕的一种，苹果公司2010年推出，将更多的像素点（彩灯）压缩到一块屏幕里，从而达到更高的分辨率。
- 苹果曾经给出一个标准：手机屏幕达到300PPI，平板屏幕达到220PPI，笔记本电脑达到200PPI即可认为是Retina屏幕

## 像素密度PPI（pix per inch）

- 每英寸的物理像素点的数量。屏幕分辨率为1080X2340的iphone12mini为例，ppi=Math.sqrt(1080^2+2340^2)/5.4，值为476PPI
  - 通常可以这么说：同样的屏幕尺寸的情况下，ppi越高，屏幕显示效果越好。

## CSS像素

- CSS语言中用来表示长度的一个单位，单位是px
  - CSS像素不能直接和现实中的长度进行换算
  - 没有缩放的情况下，CSS像素与设备独立像素保持1:1的换算比。
    - 假如一个屏幕的设备独立像素为1920X1080，那么一个宽度为960px的元素视觉上会占据屏幕一半的宽度。
    - 通常情况下修改的分辨率是显示分辨率，（屏幕分辨率是像素点的排列不可更改），也因此将屏幕分辨率改小会让元素显得更大。

## 设备像素比（DPR）以及图片的高清显示

- 设备像素比指的是物理像素与设备独立像素的比例

  - 设备独立像素由1个或N个物理像素点（彩灯）来显示，物理像素点越多，显示的图像越清晰

  - 以iphone12 Pro Max为例：物理像素为1284 X 2778，逻辑像素为428 X 926，设备像素比为3。它代表一个设备独立像素使用9个物理像素来显示。

  - `window.devicePixelRatio`来获取屏幕设备像素比

  - ```
            @media only screen and (-webkit-min-device-pixel-ratio:1.25) {
                #d1 {
                    width: 160px;
                    height: 160px;
                    background-color: red;
                }
            }
    ```

  - 上面的代码用于应对不同的设备像素比下的图片的高清显示。比如一张图片存在100X100,200X200,300X300三种规格。使用上面的代码在不同的设备像素比下使用不同的1x，2x，3x图片。

## 位图像素

- 位图：称为点阵图像或者栅格图像，由单个的像素点组成。缩放后失真。一个位图像素对应一个物理像素，图片才能得到清晰的显示。
  - 就像沙滩上用彩色沙子绘制的图像，在五百米高空中可以看到精彩的画面，但当走进这片沙滩时，会看到组成图像的每一粒沙子以及每颗沙粒单纯且不可改变的颜色。
- 矢量图：称为向量图。矢量图是由多个对象的组合生成的。每一个对象的记录方式都是以数学函数来实现的。矢量图并不记录画面上的每一点信息，而是记录元素的形状以及颜色的算法。
  - 矢量图相对于位图，在颜色的复杂度上不如位图，更擅长于更改形状。矢量图需要相应的程序才能打开和编辑。

## 像素之间的关系

- 不缩放的情况下：css像素==设备独立像素==位图像素

## 真机测试流程

- 草料二维码网站可以将URI转换为二维码进行访问

## 视口

- PC端
  - css文档里，视口又被称为初始包含块。它由浏览器的可视区域组成。是所有css百分比宽度推算的根源。
  - `document.documentElement.clientWidth`:获取浏览器可视区域宽度
  - `window.innerWidth`:获取浏览器的窗口的宽度
    - 它和可是区域的区别在于如果浏览器右侧有滚动条，那么可视区域就需要减去滚动条的宽度。窗口宽度则不需要
  - `window.outerWidth`:获取浏览器总宽
    - 它与innerWidth的区别在于：当浏览器不是全屏的时候，浏览器外部有一圈阴影。outerWidth需要加上阴影的距离。
  - `screen.width`:获取屏幕的宽度（设备独立像素）
- 移动端
  - 移动端有3个视口：
    - 布局视口、视觉视口、理想视口
    - 获取布局视口：（它用来放置网页内容）
      - `document.documentElement.clientWidth/clientHeight`
      - `ctrl-shift-i`：打开后台
      - `ctrl-shift-m`：打开移动端视图
      - `dtrl-shift-d`：把控制台从下面移到右边
      - 视口大小由浏览器厂商决定，大多数设备的布局视口是980px
      - 一般网页大小960-1200px，而手机只有375。因此浏览器厂商用了一个虚拟视口980，将网页缩小显示在手机上。目的是兼容早期的PC页面。
    - 视觉视口：能看到的网页的区域
      - `window.innerWidth/innerHeight`
      - 移动浏览器没有控制台。
      - 页面不缩放的情况下，视觉视口宽度==布局视口宽度
    - 理想视口
      - 宽度与屏幕设备独立像素同宽的布局视口称为理想视口
      - 用户不需要缩放和滚动条就能看到网站的全部内容
      - 针对移动端的设计稿更容易开发（一般设计稿都是750px）
      - `<meta name="viewport" content="width=device-width,initial-scale=1.0">`
        - meta可以设置字符集，设置关键字，设置描述，设置作者

## 缩放

- 绝大多数网页不会允许用户把网页缩放的
- 980像素是浏览器厂商设置的布局视口的初始宽度
- 放大时：
  - 布局视口与视觉视口都在变小。元素本身不变（单个css像素占据的物理像素更多了）

## 点击穿透

- touch 事件结束后会默认触发元素的 click 事件，如没有设置完美视口，则事件触发的时间间隔为 300ms 左右，如设置完美视口则时间间隔为 10ms 左右。

- 如果 touch 事件隐藏了元素，则 click 动作将作用到新的元素上，触发新元素的 click 事件或页面跳转，此现象称为点击穿透

- 解决方法(禁止触发touchustart事件后再触发click事件)

  - 阻止默认事件

    - `e.preventDefault`

  - 阻止顶级元素事件的默认行为,可以增加一个包裹元素绑定,也可以给document和window对象绑定,不过需要关闭被动模式

    - ```
            document.addEventListener('touchstart', function (e) {
                  e.preventDefault()
              }, {
                  passive: false
              })
      ```

    - 暂不清楚原理

  - holder.js的使用
  
  - 跳转页面
  
    - ```
              document.querySelector('img').addEventListener('click', function () {
                  location.href = this.dataset.href
              }, false)
      ```

## SEO

- 爬虫无法爬到网页渲染后的数据. js写的越多, 越不利. 
- 尽量少用js创建元素
- 页面跳转的选择
  - 移动端页面跳转可以使用 a 链接，也可以使用 touchstart 事件来触发 JS 代码完成跳转
  - 效率上, touchustart速度更快
  - SEO优化上, a链接效果更好

## 浏览器的默认行为

- 下拉露白

- 长按选中文字

- safari不受user-scale限制, 允许缩放

- 解决方法: 
  - 可以给 document 绑定 touchstart 事件，并阻止默认行为，不过需要关闭被动模式。<span style="color:#ee0b41">这里推荐创建一个包裹元素，绑定 touchstart 事件并阻止默认行为。</span>

  - ```
    app.addEventListener('touchstart', function(e){
    	e.preventDefault();
    })
    ```

- 后遗症: 最外层元素阻止了 touchstart 默认行为之后，会产生一些意外现象

  - 连接失效(a连接失效, 通过touchstart跳转的链接依然可用)
  - 内容无法选择
  - form 元素无法获得焦点

- 解决方法

  - 产生『后遗症』的原因在于 touchstart 阻止了默认行为，后续所有的操作都已经失效。解决问题只需要给目标元素绑定 touchstart 事件并阻止事件冒泡，这样当前操作的默认行为仍然可用。
  - 对a标签,p标签,input标签三种需要通过这方式解决

### 事件对象属性

- 检测`ctrl+c`: `if(e.keyCode === 67 && e.ctrlKey){}`

- rgb()选120以后的颜色都是暖色, 不会刺眼
- 三个特别属性
  - changedTouches
  - targetTouches
    - e.targetTouches同样返回一个伪数组, e.targetTouches[0].target获得当前点击的元素
  - touches
    - 一根手指按下时会触发touchstart事件, 打印e.touches , 得到一个TouchList伪对象. 此时的e.touches.length为1
    - 两根手指按下时会触发两次touchstart事件, 此时的e.touches.length为2
    - 移除一根手指时会触发一次touchend事件,此时的e.touches.length减1
    - e.touches[0].target可以获取到当前点击的那个元素

### touchstart 事件

- changedTouches 为当前在元素上同时按下的触点对象数组
- targetTouches 为按下后,当前元素上的触点对象数组
- touches 为按下后,当前屏幕上所有的触点对象数组

### touchmove 事件

- 对touchmove事件阻止默认事件时需要添加`{passive:false}`
- changeTouches 为当前在元素上同时滑动的触点对象数组
- targetTouches 为滑动时,当前元素上的触点对象数组
- touches 为滑动时,当前屏幕上所有触点对象数组

### touchend 事件

- changedTouches  为当前在元素上<span style="color:#ee0b41">同时抬起</span>的触点对象数组。
- targetTouches  为结束时时，当前元素上的触点对象数组
- touches  为结束时时，当前屏幕上所有的触点对象数组

### touch事件属性的总结

- e.touches获取当前屏幕上的所有触点,无论这个触点是否在当前元素上
- e.targetTouches只获取当前触发元素内部的触点. 由于touchstart等事件对象都会冒泡,因此不设定禁止冒泡的话, 触发元素内部的元素的touchstart等事件也会使得当前的e.targetTouches包含该触点
- e.changedTouches 获取的是事件触发时新增,减少或移动的触摸点的集合.



- 举例:页面上有一个div1和一个div2,只有div2绑定了touchstart事件
- 放一根手指在div2上,触发事件,此时的e.touches,e.targetTouches,e.changedTouches都只包含一个触点.
- 放下第2根手指在div1上,第3根手指在div2上.触发事件.
  - 此时的e.touches包含屏幕上所有的触点
  - 此时的e.targetTouches只包含div2上的触点,即第1个和第三个触点
  - 此时的e.changedTouches只包含第2和第3个触点

### 拖拽

- 

## 蓝湖viewport

- 下载蓝湖插件(要求cc2015版本以及以上)
- ps- 窗口 -扩展功能- 蓝湖- 登录账户- 上传全部画布
- 用途:快速自动计算设计图间距,单位是pt

- 量尺寸用蓝湖去做

## viewport适配

- 设置viewport width等于设计稿的宽度
- 量取元素的尺寸,量出来是多少就写多少.比如元素宽度100像素,则css样式为 width:100px;
- 这种方式用得少.会有兼容性问题.
- 多数情况下使用rem适配

## <span style="color:red">rem适配</span>

- em和rem

  - em相对于父元素
  - rem相对于html的font-size属性

- 设计稿大多375X1334规格,按照iphone6设计

- 淘宝的375设计图的html{font-size:100px}

- 增加js代码进行rem适配

  - ```
                function viewWidth() {
                    return window.innerWidth || document.documentElement.clientWidth
                }
    ```

  - 需要用到onresize监听视口大小的改变

- 稿子不是375怎么办?将稿子等比例缩放至375再测量. 375 的稿子的html{font-size:100px}

- html的font-size的值小于12px时是无效的.所以这个值不能过小.375稿子的font-size设置为100px是因为淘宝是这么写的.

- 或者按照类似于bootstrap的栅格布局,将html的font-size设置为视口宽度的1/10或者1/12,然后用以设置元素的宽度

## less与rem适配的结合

- 在less文件中设置 @rem:(375/10rem)

  - 将设计稿的显示样式全部视为375规格

- 元素的大小以其测量值为准设置,less文件中设置为:

  - ```
    #d1{
    	width:(200/@rem);
    	height:(100/@rem);
    }
    ```

- 在js文件中设置html的font-size属性:

  - ```
            document.documentElement.style.fontSize = document.documentElement.clientWidth / 10 + 'px'
            window.onresize = function () {
                document.documentElement.style.fontSize = document.documentElement.clientWidth / 10 + 'px'
            }
    ```

## 像素边框

- 1px边框问题: 高清屏下1px对应更多的物理像素,所以1px看起来比较粗,解决方法:

  - 方法一:使用伪类

    - ```
      ```

    - 



## 适配小实践——银行卡

## 正则替换内容

## 地图拖拽效果

- 设定一个框,要求图片随手指的滑动滚动
- 要求图片的边界不会滑动到框的外面去(边界限定)

## 布局

- 写一个用手指滑动的轮播图

![轮播图](E:\2020尚硅谷前端\008-移动端\day-04\课件\轮播图.jpg)

- 里面元素的位置不改变,外面的包裹元素的位置在改变

- 检测标准:轮播图通过touchmove控制滑动,但通过touchuend判断是否需要切换图片
- 对于touchend事件,唯一能用的只有changedTouches来获取离开的触点 位置

## 可拖拽

## 拖拽切换幻灯片

## 切换的检测-距离与事件检测

- 距离检测:如果滑动距离小于等于一半,则返回到目前的图片,否则滑动到下一张

- 计算按下触点到释放触点的时间(300ms),如果小于这个值则不进行距离检测,直接快速滑动

## 元素样式的初始化

- 因为之前的的样式是写死的375,所以需要对元素样式进行初始化,即对包裹元素的宽高进行设置

- 获取第一张幻灯片的高度然后设置其包裹元素的高度
- 图片的加载需要时间,如果在图片加载完毕之前获取图片高度可能无法获取到,保险的是在onload事件中获取
- 包裹元素的宽度设置

## 无缝滚动

- 将子元素全部复制一次,当移动第0个时,立刻将位置移动到第5个,当移动第

## translateX版本封装



## 音悦台

- css准备

	- 设置全局reset文件
	- swiper.css文件
  		- 通过@import在app.less文件中引入其它less文件
- js文件准备
	- 取消浏览器默认行为
	- 恢复超链接功能
	- trnasformCSS与swiper文件准备
- 图片文件夹拷贝
- 入口文件
  - 引入CSS与JS文件
- 使用rem实现移动端的适配

## 音悦台头部

```
头部区域颜色： #232323

头部上半部分高度:135
	
logo部分:
    log 图片大小:240px * 88px
    log 左右内边距:17px
                上内边距:26px
                下内边距:21px
                
菜单按钮:
    菜单元素    129 * 135
    菜单按钮    雪碧图大小:82 * 233
    背景图偏移位置(关闭): center 16
    背景图偏移位置(开启): center -120

按钮容器:
    上内边距   21

登录/注册按钮:(注意内联元素,需要浮动)
    按钮大小: 111 * 78
    行高:78
    背景色 :  #690
    字体颜色: #ccc
    右外边距: 15px
    字体大小: 42
    文本居中
    圆角:8
小搜索按钮:
    按钮大小: 130 * 88
    行高:88
    字体颜色: #fff
    右外边距: 30px
    上外边距: 3px
    字体加粗
    圆角:10
------------------------------------------------
搜索区:
    高	:103;
    上下左右内边距:16;

输入框:(注意boxsizing!!!)
    宽(总): 829
    高(总): 103
    背景色: #999;
    上下内边距: 5
    左右内边距: 10
    边框: 1px solid #5a5a5a;
    字体大小 : 41
    字体颜色: #333
    圆角:15
    获取焦点后，背景颜色变白
    
大搜索按钮
     宽: 203
     高: 103
     边框:清除边框
     背景颜色: #414040;
    字体颜色: #fff;
     字体大小 : 41
    圆角: 15;
-------------------------------------------------
定位层:
    宽度		 :100%
    绝对定位top : 135
    上下内边距	  : 10
    上边框: 1px solid #6a6a6a 
    背景颜色:rgba(0, 0, 0, .8)

定位层元素:
    宽度	: 22.5%
    高度:135
    行高:135
    字体大小:54
    文本居中 加粗 白色
  

```

## 移动端rem适配 

- 方法一 : 定值适配

  - 总设计稿按375布局,设计稿大多数是750x1334
  - 设置font-size 100px 尺寸转为rem

- 将html的font-size属性强行写成100px, 所有div的元素全部按照375的规格进行测量(如果拿到其它尺寸的设计稿, 那么将设计稿改为375px的规格,psd的稿子是可以改规格的), 然后将元素写成375格式下的rem单位的值.(比如150px写为1.5rem)

- 再将js中对html的font-size进行再配置, clientWidth/fs = 375/100

  - `document.documentElement.style.fontSize=document.documentElement.clientWidth*100/375+'px'`

- 方法二 : rem与less定比例适配

  - 不选用固定值作为html的font-size,而是选择一个比例值

  - 1 .将屏宽的1/10作为1rem的值 , 在less中写入 @fontSize=1080/10rem   :   1080是稿子的宽度,不需要转换直接拿来使用, 

  - 2 .然后将直接测量得到的数据直接转换成rem数据

    - ```css
      #box{
      	width:520/@fontSize; 
      	height: 335/@fontSize
      	}
      ```

  - 3 .使用js将html的内联font-size属性设置为屏宽的1/10

## 导航条布局+拖拽

```html
<div id="nav">
	<ul class="wrapper">
        <li></li>
        <!-- 一堆li元素 -->
    </ul>
</div>
```

- 结构没什么好说的,一个width100%的div包裹框包裹着ul.wrapper元素

## 导航条的惯性移动和回弹效果

- touchend事件中需要达到效果:
  - 点击跟随移动
  - 

### 水平撑开元素的方法

- 通过让子代li元素{dispaly:ibnline-block;},和ul{font-size:0;position:absolute;}达到横向排版的目的.
- 绑事件(touch事件)给最外层包裹元素绑事件

### 贝塞尔曲线

- `cubic-bezier(0.08, 1.44, 0.6, 1.46)`
  - 使用贝塞尔曲线来达到回弹的效果
  - 贝塞尔曲线 https://cubic-bezier.com/

### 惯性移动

- 先求速度,再乘以系数,得到惯性位移

## 橡皮筋效果

- 首选判断边界,通过transformCSS封装函数获取偏移量
  - 如果大于0或者小于`oContainer.offsetWidth-oWrapper.offsetWidth`,那么多出的偏移量减半.
  - 如果没有越界,oWrapper的位移应该是触点间的位移

## 点击切换导航--导航误触误滑问题解决

- 问题:滑动的情况下会触发touchstart事件更改掉li的类
- 解决:在touchend事件根据touchmove事件来判断是否滑动,没有滑动则不会触发touchmove事件.

```javascript
    oBrandNavWrapper.addEventListener('touchend', function (e) {
        if (!isMove) {
            aLi.forEach((item, index, arr) => {
                item.classList.remove('active')
            })
            if (e.target.tagName === 'A') {
                e.target.parentNode.classList.add('active')
            }
        } else {
            isMove = false
            return
        }
    }, false)
```

## 内容区布局+轮播切换结果

- 没什么好记录的,按照图片写就行了

## 内容区导航切换--幻灯片切换

- 讲如何完成点击导航切换幻灯片

- 得到导航条元素的序数.
  - 可以直接在html结构中添加属性index或者data-index
  - 可以用for或者forEach为element对象添加index属性
  - 因为通过变量获得了`new Swiper()`的对象oContainer,也因此可以获得init,toMove等方法,所以在点击导航条移动时可以直接利用toMove,没必要写其它代码.

## 幻灯片切换--底部移动边框切换

- 这一章讲如何解决底部的那条小横线的移动

- ```javascript
  this.getIndex=function(){
  	return iNow
  }
  ```

- 在swiper.js中添加上面的代码,在callback.end中获取当前的iNow,然后再移动小横线

## MV的加载效果

- 删掉所有的.swiper-slide中的内容,只保留第一个

- 在其它的.swiper-slide中添加

  - ```html
    <div class="loading"></div>
    ```

  - ```css
    .loading{
        width:100%;
        height:101/@font-size;
        background-image:url('..');
        background-repeat:no-repeat;
        backgounde-position:center;
    }
    ```

- 在callback.end中添加代码

  - ```javascript
    setTimeout(()=>{
    	// 获取第一张轮播图的内容
        // 再复制代码进当前的幻灯片里
    },2000)
    ```
  
- 判断.swiper-slide是否已经被替换内容

- 把callback.end的调用放在toMove函数中而不是放在touchend的事件监听回调中.

## ajax请求演示-fail

- 在页面不刷新的情况下向服务器发起请求
- url结构
  - 协议http
  - 域名
- 失败:服务器并没有开放权限

## tweenAnimation

### tween的认识

```javascript
var Tween = {
    Linear: function(t,b,c,d){ return c*t/d + b; },
    Quad: {
        easeIn: function(t,b,c,d){
            return c*(t/=d)*t + b;
        },
        easeOut: function(t,b,c,d){
            return -c *(t/=d)*(t-2) + b;
        },
        easeInOut: function(t,b,c,d){
            if ((t/=d/2) < 1) return c/2*t*t + b;
            return -c/2 * ((--t)*(t-2) - 1) + b;
        }
    },
    Cubic: {
        easeIn: function(t,b,c,d){
            return c*(t/=d)*t*t + b;
        },
        easeOut: function(t,b,c,d){
            return c*((t=t/d-1)*t*t + 1) + b;
        },
        easeInOut: function(t,b,c,d){
            if ((t/=d/2) < 1) return c/2*t*t*t + b;
            return c/2*((t-=2)*t*t + 2) + b;
        }
    },
    Quart: {
        easeIn: function(t,b,c,d){
            return c*(t/=d)*t*t*t + b;
        },
        easeOut: function(t,b,c,d){
            return -c * ((t=t/d-1)*t*t*t - 1) + b;
        },
        easeInOut: function(t,b,c,d){
            if ((t/=d/2) < 1) return c/2*t*t*t*t + b;
            return -c/2 * ((t-=2)*t*t*t - 2) + b;
        }
    },
    Quint: {
        easeIn: function(t,b,c,d){
            return c*(t/=d)*t*t*t*t + b;
        },
        easeOut: function(t,b,c,d){
            return c*((t=t/d-1)*t*t*t*t + 1) + b;
        },
        easeInOut: function(t,b,c,d){
            if ((t/=d/2) < 1) return c/2*t*t*t*t*t + b;
            return c/2*((t-=2)*t*t*t*t + 2) + b;
        }
    },
    Sine: {
        easeIn: function(t,b,c,d){
            return -c * Math.cos(t/d * (Math.PI/2)) + c + b;
        },
        easeOut: function(t,b,c,d){
            return c * Math.sin(t/d * (Math.PI/2)) + b;
        },
        easeInOut: function(t,b,c,d){
            return -c/2 * (Math.cos(Math.PI*t/d) - 1) + b;
        }
    },
    Expo: {
        easeIn: function(t,b,c,d){
            return (t==0) ? b : c * Math.pow(2, 10 * (t/d - 1)) + b;
        },
        easeOut: function(t,b,c,d){
            return (t==d) ? b+c : c * (-Math.pow(2, -10 * t/d) + 1) + b;
        },
        easeInOut: function(t,b,c,d){
            if (t==0) return b;
            if (t==d) return b+c;
            if ((t/=d/2) < 1) return c/2 * Math.pow(2, 10 * (t - 1)) + b;
            return c/2 * (-Math.pow(2, -10 * --t) + 2) + b;
        }
    },
    Circ: {
        easeIn: function(t,b,c,d){
            return -c * (Math.sqrt(1 - (t/=d)*t) - 1) + b;
        },
        easeOut: function(t,b,c,d){
            return c * Math.sqrt(1 - (t=t/d-1)*t) + b;
        },
        easeInOut: function(t,b,c,d){
            if ((t/=d/2) < 1) return -c/2 * (Math.sqrt(1 - t*t) - 1) + b;
            return c/2 * (Math.sqrt(1 - (t-=2)*t) + 1) + b;
        }
    },
    Elastic: {
        easeIn: function(t,b,c,d,a,p){
            if (t==0) return b;  if ((t/=d)==1) return b+c;  if (!p) p=d*.3;
            if (!a || a < Math.abs(c)) { a=c; var s=p/4; }
            else var s = p/(2*Math.PI) * Math.asin (c/a);
            return -(a*Math.pow(2,10*(t-=1)) * Math.sin( (t*d-s)*(2*Math.PI)/p )) + b;
        },
        easeOut: function(t,b,c,d,a,p){
            if (t==0) return b;  if ((t/=d)==1) return b+c;  if (!p) p=d*.3;
            if (!a || a < Math.abs(c)) { a=c; var s=p/4; }
            else var s = p/(2*Math.PI) * Math.asin (c/a);
            return (a*Math.pow(2,-10*t) * Math.sin( (t*d-s)*(2*Math.PI)/p ) + c + b);
        },
        easeInOut: function(t,b,c,d,a,p){
            if (t==0) return b;  if ((t/=d/2)==2) return b+c;  if (!p) p=d*(.3*1.5);
            if (!a || a < Math.abs(c)) { a=c; var s=p/4; }
            else var s = p/(2*Math.PI) * Math.asin (c/a);
            if (t < 1) return -.5*(a*Math.pow(2,10*(t-=1)) * Math.sin( (t*d-s)*(2*Math.PI)/p )) + b;
            return a*Math.pow(2,-10*(t-=1)) * Math.sin( (t*d-s)*(2*Math.PI)/p )*.5 + c + b;
        }
    },
    Back: {
        easeIn: function(t,b,c,d,s){
            if (s == undefined) s = 1.70158;
            return c*(t/=d)*t*((s+1)*t - s) + b;
        },
        easeOut: function(t,b,c,d,s){
            if (s == undefined) s = 1.70158;
            return c*((t=t/d-1)*t*((s+1)*t + s) + 1) + b;
        },
        easeInOut: function(t,b,c,d,s){
            if (s == undefined) s = 1.70158; 
            if ((t/=d/2) < 1) return c/2*(t*t*(((s*=(1.525))+1)*t - s)) + b;
            return c/2*((t-=2)*t*(((s*=(1.525))+1)*t + s) + 2) + b;
        }
    },
    Bounce: {
        easeIn: function(t,b,c,d){
            return c - Tween.Bounce.easeOut(d-t, 0, c, d) + b;
        },
        easeOut: function(t,b,c,d){
            if ((t/=d) < (1/2.75)) {
                return c*(7.5625*t*t) + b;
            } else if (t < (2/2.75)) {
                return c*(7.5625*(t-=(1.5/2.75))*t + .75) + b;
            } else if (t < (2.5/2.75)) {
                return c*(7.5625*(t-=(2.25/2.75))*t + .9375) + b;
            } else {
                return c*(7.5625*(t-=(2.625/2.75))*t + .984375) + b;
            }
        },
        easeInOut: function(t,b,c,d){
            if (t < d/2) return Tween.Bounce.easeIn(t*2, 0, c, d) * .5 + b;
            else return Tween.Bounce.easeOut(t*2-d, 0, c, d) * .5 + c*.5 + b;
        }
    }
}
```

- 上面的这些代码都是用来计算在特定的动画中，在指定的时刻里元素应该所在的位置
- 以linear为例：参数有t，b，c，d；以在5000ms内向右平移500px为例
  - t代表当前时间；这个值是一个变化值，在interval中不断增加，获取到对应的所在位置。
  - b代表初始值；在示例中为0
  - c代表相对变化值,在示例中代表位移；在示例中为500
  - d代表持续时间；在示例中为5000

### tweenAnimation动画函数封装



### 停止定时器

- 在tweenAniamtion的封装函数中,为了达到通过点击达到暂停的效果,则必须有:

- ```javascript
  oDiv.onclick=function(){
  	clearInterval(timer)
  }
  ```

- 引出问题:如何得到timer?

  - 将timer赋值给参数el作为属性,然后通过el元素来获取

  - ```javascript
    oDiv.onclick=function(){
    	clearInterval(this.timer)
    }
    ```

  - 又引出问题:如果同一个元素存在两个tweenAnimation的调用,那么它的el.timer属性值将会是第二个的setInterval()的返回值,也就是说只能停止第二个tweenAnimation的动画,但是第一个动画无法停止.

### 定时器冲突的解决

- 将el.timer设置为空对象值{},同时要注意前提条件,在el.timer===unbdefined时才允许定义el.timer.

  - 否则会造成效果是每次调用tweenAnimation函数都要重新定义el.timer

- setInterval()的返回值保存在el.timer[style]中.

- 通过点击事件清除所有动画示例:

  - ```javascript
            tweenAnimation(oD1, 'translateX', 0, 800, 10000, 60, 'linear')
            tweenAnimation(oD1, 'translateY', 0, 400, 10000, 60, 'linear')
          
            oD1.onclick = function (e) {
                // 清除所有的动画
                for (const prop in this.timer) {
                    clearInterval(this.timer[prop])
                }
            }
    ```

### 竖向滑屏即点即停效果

- 写一个scroll.html文件
- css文件写上{overflow:hidden}不允许出现滚动条,有也应该是制作者自己的
- 通过js写一个`1<br>2<br>3<br>`这种的文件,一直到100
- 绑定事件touchstart,通过e.touches[0].clientY获取触点距离页面顶部的位置;通过transformCSS或者this.offsetTop获取页面在y轴上的偏移量
- 绑定touchmove事件,通过e.touches[0].clientY获取触点现在距离页面顶部的位置;通过`触点当前距离页面顶部的位置-触点初始距离页面顶部的位置+页面本身的偏移位置`获得当前应该偏移的位置
- 绑定touchend事件,进行边界检测,当偏移量大于0或者小于`容器长度减去页面长度`时返回到边界位置.
- 写惯性移动效果:距离除以时间获取到平均速度,然后touchend事件触发后偏移量再加上v*t的结果,通过transition过渡这段距离
  - 获取平均速度的过程不作叙述,速度的单位是px/ms
  - 计算最终的位置:触点移动位置+v*t,这里的t取120ms.
  - touchstart事件移除过渡,end事件添加过渡  
- 但是惯性移动特效无法实现即点即停.移动后再次点击时立刻取消transition过渡,页面立即跳转到终点位置.
- 注释掉touchstart的transition='none'代码,注释掉touchend的transition代码,已知要移动的距离,要移动的起始位置,要移动的时间,通过tweenAnimtion来写惯性移动效果
- 在touchstart事件中清除掉动画,达到即点即停效果.但要注意判断timer存在且timer的相关属性存在(这里的属性是translateY),否则第一次会报错.

### 滚动回弹效果——滚动条跟随滚动

- 在touchend事件中声明一个变量type=‘easeOut’，在边界判断条件语句中改写这个变量type="backEaseOut"(s参数是回弹系数)

- 自己写一个滚动条。在页面中写一个`<div class="scroll-bar">`

  - 写入css文件

    - ```css
      .scroll-bar{
      	position:absolute;
      	right:0;
      	top:0;
      	width:6px;
      	border-radius:3px;
      	height:100px;
      	background:rgba(0,0,0,0.8);
      }
      ```

  - 滑动条的高度应该是通过js文件来进行设置

    - 滚动条的高度：当前视口的高度=当前视口的高度：文档的高度

    - ```javascript
      app.init=function(){
      	var h=app.offsetHeight**2/content.offsetHeight
      	scrollBar.style.height=h+'px'
      }
      ```

  - 在touchmove中写入代码：

    - 计算滚动条的位置：

      - 滚动条的translateY的值：视口的高度(appHeight)=文档的translateY(app.translateY)：文档的高度(contentHeight)

      - 

        ```javascript
        var appHeight=app.offsetHeight
        var contentHeight=content.offsetHeight
        var y= this.translateY/contentHeight*appHeight
        transformCSS(scrollBar,'translateY',-y)
        ```

    - 出现问题：超过边界的时候显示错误。超过了

      - 在end里计算滚动条的translateY

        - ```javascript
          var y=finallyY/content.offsetHeight*app.offsetHeight
          ```

        - finally是end时的e.targetTouches[0].clientY-this.startY+this.startTop+惯性移动的距离

        - 通过tweenAnimation使滚动条移动.注意type滚动时是'easeIn',超过边界滚回时是'easeOut'

        - touchStart事件中需要清理所有的动画

  - 补充:

    - 滚动回弹的代码:只是简单地根据边界判断修改tweenAnimation的type参数

    - ```
      if(translateY>=0){
      	translateY=0
      	type='backEaseOut'
      }
      ```

    - ```
      if(minTranslateY<=minTranslateY){
      	translateY=minTranslateY
      	type="backEaseOut"
      }
      ```

  - 写滚动条:

    - html结构

      - ```
        <div id="app">
        	<div id="content"></div>
        	<div class="scroll-bar">
        </div>
        ```

    - CSS

      - ```
        .scroll-bar{
        	position:absolute;
        	right:0;
        	top:0;
        	width:6px;
        	border-radius:3px;
        	height:100px;
        	background:rgba(0,0,0,0.8);
        }
        ```

    - js

      - ```
        //初始化,计算滚动条的高度
        app.init=function(){
        	var h=app.offsetHeight**2/content.offsetHeight
        	scrollBar.style.height=h+'px'
        }
        ```

      - ```
        // touchmove
        var appHeight=app.offsetHeight
        var contentHeight=content.offsetHeight
        var x=this.translateY / contentHeight * appHeight
        transformCSS(scrollBar,'translateY',y)
        ```

      - 

      - 滚动条的高度与屏幕高度存在比例: 滚动条高度/视口高度=视口高度/文档高度

      - 滚动条的translateY值与文档的translateY值存在以下条件:

        - 文档的translateY值/文档高度 = -滚动条的translateY值/视口高度
        - 文档的translateY值是负值，滚动条的translateY值是正值

      - ```
        // touchend
        // 重新计算滚动条的translateY
        var y = -translateY / content.offsetHeight * app.offsetHeight
        var currentScrollBarTranslateY=transformCSS(scrollBar,'translateY')
        // 这一条放置在代码块的最后才能获取到type值
        tweenAniamtion(scrollBar,'translateY',currentScrollBarTranslateY,Y,500,60,type)
        ```

      - 出现问题：拉取过界后滚动条无法归位

        - 将translateY的过界判断提前在计算滚动条的位置之前

        - ```
          if(translateY>=0){
          	translateY=0
          	type='backEaseOut'
          }
          if(minTranslateY<=minTranslateY){
          	translateY=minTranslateY
          	type="backEaseOut"
          }
          var y = -translateY / content.offsetHeight * app.offsetHeight
          var currentScrollBarTranslateY=transformCSS(scrollBar,'translateY')
          tweenAniamtion(scrollBar,'translateY',currentScrollBarTranslateY,Y,500,60,type)
          ```

        - 

  ### touchScroll函数的封装

  - 封装一个构造函数,名字叫touchScroll.实现触摸滑动

  - 调用示例:

    - ```html
      <div id="container">
      	<div class="wrapper"></div>
      </div>
      ```

    - ```javascript
      new TouchScroll('#container','.wrapper',{
          bg:'',
          width:
      })
      ```
  
  - ```
    function TouchScroll(container,content){
    
    }
    ```
  
  - 注意通过js 创建滚动条.

### 多楼层功能实现

- 直接在index.html中引入文件

  - `transformCSS.js`,`swiper.js`,`tweenAnimation.js`,`touchscroll.js`,`app.js`

  - 在app.js文件中的内容区中创建滚动条

    - ```
      var touchscroll=new Touchscroll('#app')
      ```

  - 发现问题:`swiper.js`文件中如果传入selector是字符串，那么通过`document`来获取，但是多个相同html结构的情况下只有第一个能用。

    - 对传入的selector进行判断

    - ```
      if(typeof selector === 'object'){
      	container=selector
      }
      if(typeof selector === 'string'){
      	container=document.querySelector(selector)
      }
      
      if(container === null){
      	return console.error('容器不能为空')
      }
      ```

### 内容滑动与幻灯片bug的解决

- 问题：上下滑动的时候，如果在左右距离上也有偏移，那么会存在滑动的时候又在左右又在上下滑动
- 解决：阻止冒泡
  - 判断是否是水平方向滑动，如果是就阻止冒泡
  - 因为oApp也同样有touchmove事件的监听

## 简易的ajax版本

```javascript
                    setTimeout(() => {
                        let oFirstSwiperSlide = aFloor[i].querySelector('.swiper-slide')
                        let oSlide = aSlide[index]
                        if (oSlide.children.length === 1) {
                            let url = ""
                            let oCloneSlide = oFirstSwiperSlide.cloneNode(true)
                            $.get(url, function (data) {
                                for (let i = 0; i < data.song_list.length; i++) {
                                    let oImg = oCloneSlide.querySelectorAll('img')[i]
                                    oImg.src = data.song_list[i]
                                    let oTitle = oCloneSlide.querySelectorAll('.content-header')[i]
                                    oTitle.innerText = data.song_list[i].title
                                    oWrapper.replaceChild(oCloneSlide,oSlide)
                                }
                            },'json')
                        }
                    }, 2000);
```

- 因为接口状态码500,所以只是模仿写了一下

## 懒加载思路分析

- 是页面的一种加载效果,能够提高页面的加载速度
- 如果一个网站有上百张图片,那么等到全部图片加载完成后才显示网站的话会等很长时间

### 图片的接口

https://picsum.photos/id/2/400/300
https://picsum.photos/id/151/400/300

- 150不能用

### 如何让浏览器不加载图片

- 在img的src属性上做文章
  - 删除src属性
  - 自定义一个属性来保存src对应的值
  - 都是为了清空img的src的属性值

## 数据仓库初始化----动态创建元素----数据分页实现

- 只有16张图片,需要使其达到100张图片的效果,每16张一组进行重复展示

## 动态创建li----滑动过程中实现懒加载

```html
<!-- html结构 -->
<main>
	<div id="content">
        <ul id="imgList">
            
        </ul>
        <div class="pull-up-update">
            
        </div>
    </div>
</main>
```

- 首先取消所有img的src,把src属性添加到li元素的data-src中
- 对所有的li元素,计算其相对父元素的上偏移,计算#content的上偏移
  - 如果当前li元素相对父元素的上偏移<=main的高度+负的#content的上偏移,那么这个元素是应该显示的元素
- 在main的touchmove事件中,对每一个li元素进行检测
  - 不行的原因:对已经加载的图片修改src属性会造成再次加载,增大了请求量.
  - 修改:对li元素添加isloaded属性,已经加载的li元素禁止再次修改
- 滚动条不见:
  - `new TouchScroll()`调用的时候,ul里面没有任何元素,因此其高度为0,得到的滚动条高度为infinite.
  - 解决:对content的高度进行resize监视
