[toc]

## 错误详情

- 存在如下html结构

  - ```html
    <div id="container">
    	<ul id="nav">
            <li></li>
            <li></li>
        </ul>
    </div>
    ```

  - ```css
    #container{
        width:100%;
        overflow:hidden;
    }
    #nav{
        white-space:nowrap;
    }
    li{
        width:100px;
        height:100px;
        display:inline-block;
    }
    ```

  - 存在问题：获取#nav的宽度与#container的宽度时，（做横向导航，#nav宽度应该大于#container），获取到的是一样的值

## 解决

- #nav是一个块元素，不设定宽度的情况下它的宽度默认是100%，无法超过视口宽度。内部子元素的宽度大于#nav时也不会将它的宽度扩张，只会溢出渲染。子元素能撑开父元素宽度和高度的情况只有absolute和fixed