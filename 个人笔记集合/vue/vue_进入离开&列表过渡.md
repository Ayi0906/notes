[Toc]

# 最简单的 隐藏/出现 过渡

- ```html
      <transition name="demo">
        <div class="d1" v-if="flag"></div>
      </transition>
  ```

- ```css
  .demo-enter { // v-show=true 时的初始状态
    opacity: 0;
  }
  
  .demo-enter-to { // v-show=true 时的最终状态
    opacity: 1;
  }
  
  .demo-enter-active { // v-show=true 时初始状态和最终状态间的间隔
    transition: 1s;
  }
  
  .demo-leave{
    opacity: 1;
  }
  
  .demo-leave-to{
    opacity: 0;
  }
  
  .demo-leave-active{
    transition: 1s;
  }
  ```

- 牢记六个类名

  - `[]-enter` , `[]-enter-to` , `[]-enter-active`, `[]-leave`, `[]-leave-to`, `[]-leave-active`

- ![transition](C:\Users\董磊\Desktop\文件夹\image暂存\transition.png)

- 必须要设置`<transition>`的name属性, 才能使用自定义类名

# 给过渡设置更多的样式

- ```html
      <button @click="flag=!flag">click</button>
      <transition name="demo">
        <div class="d1" v-show="flag"></div>
      </transition>
  ```

- ```css
  .demo-enter,.demo-leave-to{
    transform: translateX(200px);
    opacity: 0;
  }
  
  .demo-enter-active{
    transition: all .3s ease;
  }
  .demo-leave-active{
    transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  ```

- 语言叙述: 

  - div在显示开始时首帧位于x:200的位置, 且opacity:0, 这个样式来源于`v-enter`
  - 然后进入显示阶段过渡, 立刻删除`v-enter`样式, 设置`v-enter-active`与`v-enter-to`样式
  - 然后显示阶段尾帧位于它原本应该处于的位置, 并且opacity变为本应该的值, 然后删除所有的enter阶段的样式
  - 然后当进入消失阶段时, 首帧位于它本来该处于的位置, 并且opacity为它本应该的值, 这个样式来源于`v-leave`
  - 然后进入消失阶段过渡, 立刻删除`v-leave`样式, 添加`v-leave-to`和`v-leave-active`样式
  - 然后消失阶段尾帧位于x:200的位置, 且opacity:0, 删除所有leave阶段样式

- enter阶段一开始就会删除`v-enter`类名, leave阶段一开始就会删除`v-leave`类名



# css动画

- 与过渡类似, 区别是在动画中 `v-enter` 类名在节点插入 DOM 后不会立即删除，而是在 `animationend` 事件触发时删除。

  - 这个结论和测试不一致, 在测试时并没有v-enter的类名, 与过渡一样, 进入过渡/动画阶段就被删除了

- ```html
      <button @click="flag=!flag">click</button>
      <transition name="demo">
        <div class="d1" v-show="flag"></div>
      </transition>
  ```

- ```css
  .demo-enter-active {
    animation: bounce-in 20s;
  }
  
  .demo-leave-active {
    animation: bounce-in 20s reverse;
  }
  
  @keyframes bounce-in {
    0% {
      transform: scale(0);
    }
    50% {
      transform: scale(1.5);
    }
    100% {
      transform: scale(1);
    }
  }
  ```

- 写动画某种程度上来说只需要写v-enter-active与v-leave-active就可以了, 首尾帧完全可以在0%与100%阶段写



# 自定义过渡类名

- 这种是在用于引入其它第三方库, 第三方库的过渡动画有固定的类名, 直接将这个类名赋值给各个阶段的类名

- 这里是用的在html中引入的animate.css 3版本, 4版本有些毛病不能用

- ```html
      <transition name="demo" enter-active-class="animated bounceInUp" leave-active-class="animated bounceOutDown">
        <div class="d1" v-show="flag"></div>
      </transition>
  ```

- 这里的这个name属性似乎已经没必要设置了, 我本以为这个能够复用, 但是再次用`<transition name="demo">`来包裹一个div并没有起作用





# 使用钩子函数来制作动画

- 存在八个钩子函数, 每个阶段四个钩子函数

- ```html
  <transition
    v-on:before-enter="beforeEnter"
    v-on:enter="enter"
    v-on:after-enter="afterEnter"
    v-on:enter-cancelled="enterCancelled"
  
    v-on:before-leave="beforeLeave"
    v-on:leave="leave"
    v-on:after-leave="afterLeave"
    v-on:leave-cancelled="leaveCancelled"
  >
    <!-- ... -->
  </transition>
  ```

- ```js
  ```

- 

