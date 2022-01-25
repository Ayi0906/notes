[Toc]

## em框

- 在《css权威指南》（3th）中关于font-size属性的描述：
  - 字体本身有一个em框.
  
  - 样式表里的font-size属性实际上设置的是em框的高度。
  
    - ```
          <span>a</span><span>b</span><span>c</span><span>d</span><span>e</span><span>A</span><span>B</span><span>C</span><span>D</span><span>E</span>
      ```
  
    - ```
              span {
                  background-color: rgb(195, 58, 69);
                  margin: 0 5px;
                  font-size: 100px;
              }
      ```
  
    - ![image-20211013163752922](C:\Users\董磊\AppData\Roaming\Typora\typora-user-images\image-20211013163752922.png)
  
    - 上面的代码在chrome浏览器中每个span元素都生成了一个高度为132px的inline-box，
  
  - 字体的实际大小与em方框没有绝对关系，em框并不是字体的边界。但是一般来说字体都会设计得比em框小。
  
  - 