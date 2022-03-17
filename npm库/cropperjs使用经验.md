# 安装

`npm i cropperjs`

## 使用

### html架构

```
<div>
	<img>
</div>
```

### css样式

```
/* 确保图像的大小与容器完全匹配 */
img {
  display: block;

  /* 很重要 */
  max-width: 100%;
}
```

- div限制了选择的图片可以显示多大，也限制了裁剪框的大小



### 新建cropper类

```
            let cropper // 将其放在全局下是为了方便销毁后重建
            // 省略一大堆代码
            function build() {
                let image = $('#cropImg').get(0)
                cropper = new Cropper(image, {
                    aspectRatio: 1 / 1, //默认比例
                    preview: '.previewImg', //预览视图
                    guides: true, //裁剪框的虚线(九宫格) false没有九宫格，true有
                    autoCropArea: 0.5, //0-1之间的数值，定义自动剪裁区域的大小，默认0.8
                    movable: false, //是否允许移动图片
                    dragCrop: true, //是否允许移除当前的剪裁框，并通过拖动来新建一个剪裁框区域
                    movable: true, //是否允许移动剪裁框
                    resizable: true, //是否允许改变裁剪框的大小
                    zoomable: true, //是否允许缩放图片大小
                    mouseWheelZoom: false, //是否允许通过鼠标滚轮来缩放图片
                    touchDragZoom: true, //是否允许通过触摸移动来缩放图片
                    rotatable: true, //是否允许旋转图片
                    viewMode: 2, //不允许裁剪框超出图片本身
                    crop(event) {
                        console.log(event.detail.x);
                        console.log(event.detail.y);
                        console.log(event.detail.width);
                        console.log(event.detail.height);
                        console.log(event.detail.rotate);
                        console.log(event.detail.scaleX);
                        console.log(event.detail.scaleY);
                    }
                });
            }
```

- 首先获取到div下的图片，复制cropper类的配置，viewMode我觉得很实用，crop下的一大堆其实可以省略掉。

- 所有的方法都是通过cropper.xxx()来调用的。

  #### 每次打开都新建cropper

  #### 每次关闭裁剪框都销毁cropper

  - .desytroy()

  - 因为当我选中一张图片，并使之显示在裁剪框中后，点击隐藏裁剪框，打开后图片依然存在，也因此使用.destroy()来在关闭时销毁裁剪框。

    ```
                function close() {
                    console.log(`销毁cropper`)
                    cropper.destroy()
                }
    ```

  ### 重置、旋转、镜像

  - 重置：cropper.reset()

    - 和想象的不一样，只是将图片放置在最初的样子。

  - 旋转：cropper.rotate(90)

    - 接收一个数字，90代表顺时针旋转90度。

  - 镜像：cropper.scaleX()

    - ```
                  let flagX = true
                  $(".btn-scaleX").click(function () {
                      if (flagX) {
                          cropper.scaleX(-1)
                          flagX = false
                      } else {
                          cropper.scaleX(1)
                          flagX = true
                      }
                  })
      ```

  ### 裁剪后的上传

  ```
                      cropper.getCroppedCanvas()
                          .toBlob((blob) => {
                              let formData = new FormData()
                              formData.append("croppedImage", blob, 'cropImg.webp')
                              console.log(formData.get("croppedImage"))
                              $.ajax('/upload', {
                                  type: 'POST',
                                  cache: false,
                                  data: formData,
                                  processData: false, // 告诉jquery不要去处理发送的数据
                                  contentType: false, // 告诉jquery不要去设置Content-type的请求头
                                  success() {
                                      console.log('Upload success')
                                  },
                                  error() {
                                      console.log('Upload error')
                                  }
                              })
                          }, "image/webp", 1)
  ```

  - 通过cropper.getCroppedCanvas()方法获取到裁剪后的canvas图片，然后再将其转换为blob文件，通过FormData.append()添加后post到服务器中（2021.8.19：post失败，前端根本没有发送正确的数据，问题不知道在何处。服务器接收空对象）
