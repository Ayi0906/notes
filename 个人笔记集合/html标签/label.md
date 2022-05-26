- `label`可以与`input`挨着写, 此时需要id和for属性

  - ```html
        <label for="peas">Do you like peas?</label>
        <input type="checkbox" name="peas" id="peas">
    ```

- 也可以直接把`input`标签写进`label`里面, 此时不需要id和for属性

  - ```html
    <label>Do you like peas?
      <input type="checkbox" name="peas">
    </label>
    ```

- 结构上`label`是行内元素

- 如果label内的文本需要做视觉上的调整, 那么不应该为文本本身设定样式, 而是要对`label`设定样式

  - 错误:

    - ```html
      <label for="your-name">
        <h3>Your name</h3>
        <input id="your-name" name="your-name" type="text">
      </label>
      ```

  - 正确

    - ```html
      <label class="large-label" for="your-name">
        Your name
        <input id="your-name" name="your-name" type="text">
      </label> 
      ```

    - 