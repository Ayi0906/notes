## 网页引入

```html
<link  href="/path/to/cropper.css" rel="stylesheet">
<script src="/path/to/cropper.js"></script>
```

## 使用

```javascript
new Cropper(element[, options])
```

- element:

  - 格式：HTMLImageElement or HTMLCanvasElement

  - 说明：要裁剪的图片元素或画布元素

## option:

### viewMode（视图模式）
- Number格式
- 默认值：0
- 0:没有限制
- 1：限制裁剪框不超过画布的大小。
- 2：限制最小画布大小以适合容器。如果画布和容器的比例不同，则最小画布将被其中一个维度中的额外空间包围。
- 3：限制最小画布大小以填充容器。如果画布和容器的比例不同，则容器将无法在其中一个维度中容纳整个画布。
- 定义裁剪器的视图模式。如果将viewMode设置为0，裁剪框可以延伸到画布之外，而值1、2或3将限制裁剪框到画布的大小。viewMode为2或3将另外将画布限制为容器。当画布和容器的比例相同时，2和3之间没有区别。

### dragMode（拖动模式）
    - Type:字符串格式
    - Default: `'crop'`
    - Options:
      - `'crop'`: 创建新的裁剪框
      - `'move'`: 移动画布
      - `'none'`: do nothing
    - 定义裁剪器的拖动模式。
### initialAspectRatio（初始纵横比）

- Type: `Number`
- Default: `NaN`

定义裁剪框的初始纵横比。默认情况下，它与画布（图像包装器）的纵横比相同。

> 仅当'aspectRatio'选项设置为'NaN'时可用。

### aspectRatio（纵横比）

- Type: `Number`
- Default: `NaN`

定义裁剪框的固定纵横比。默认情况下，裁剪框具有自由比例。

### data（数据）

- Type: `Object`
- Default: `null`

初始化时，存储的上一个裁剪数据将自动传递给“setData”方法。

> 仅当'autoCrop'选项设置为'true'时可用。

### preview（预览）

- Type: `Element`, `Array` (elements), `NodeList` or `String` (selector)
- Default: `''`
- 元素或元素数组或节点列表对象或[Document.querySelectorAll]的有效选择器

为预览添加额外的元素（容器）。

**Notes:**

- 最大宽度是预览容器的初始宽度。
- 最大高度是预览容器的初始高度。
- 如果设置“aspectRatio”选项，请确保为预览容器设置相同的纵横比。
- 如果预览显示不正确，请将 `overflow: hidden` 样式设置为预览容器。

### responsive

- Type: `Boolean`
- Default: `true`

Re-render the cropper when resizing the window.

### restore

- Type: `Boolean`
- Default: `true`

Restore the cropped area after resizing the window.

### checkCrossOrigin

- Type: `Boolean`
- Default: `true`

Check if the current image is a cross-origin image.

If so, a `crossOrigin` attribute will be added to the cloned image element, and a timestamp parameter will be added to the `src` attribute to reload the source image to avoid browser cache error.

Adding a `crossOrigin` attribute to the image element will stop adding a timestamp to the image URL and stop reloading the image. But the request (XMLHttpRequest) to read the image data for orientation checking will require a timestamp to bust cache to avoid browser cache error. You can set the `checkOrientation` option to `false` to cancel this request.

If the value of the image's `crossOrigin` attribute is `"use-credentials"`, then the `withCredentials` attribute will set to `true` when read the image data by XMLHttpRequest.

### checkOrientation

- Type: `Boolean`
- Default: `true`

Check the current image's Exif Orientation information. Note that only a JPEG image may contain Exif Orientation information.

Exactly, read the Orientation value for rotating or flipping the image, and then override the Orientation value with `1` (the default value) to avoid some issues ([1](https://github.com/fengyuanchen/cropper/issues/120), [2](https://github.com/fengyuanchen/cropper/issues/509)) on iOS devices.

Requires to set both the `rotatable` and `scalable` options to `true` at the same time.

**Note:** Do not trust this all the time as some JPG images may have incorrect (non-standard) Orientation values

> Requires [Typed Arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray) support ([IE 10+](https://caniuse.com/typedarrays)).

### modal

- Type: `Boolean`
- Default: `true`

Show the black modal above the image and under the crop box.

### guides

- Type: `Boolean`
- Default: `true`

Show the dashed lines above the crop box.

### center

- Type: `Boolean`
- Default: `true`

Show the center indicator above the crop box.

### highlight

- Type: `Boolean`
- Default: `true`

Show the white modal above the crop box (highlight the crop box).

### background

- Type: `Boolean`
- Default: `true`

Show the grid background of the container.

### autoCrop

- Type: `Boolean`
- Default: `true`

Enable to crop the image automatically when initialized.

### autoCropArea

- Type: `Number`
- Default: `0.8` (80% of the image)

It should be a number between 0 and 1. Define the automatic cropping area size (percentage).

### movable

- Type: `Boolean`
- Default: `true`

Enable to move the image.

### rotatable

- Type: `Boolean`
- Default: `true`

Enable to rotate the image.

### scalable

- Type: `Boolean
- Default: `true`

Enable to scale the image.

### zoomable

- Type: `Boolean`
- Default: `true`

Enable to zoom the image.

### zoomOnTouch

- Type: `Boolean`
- Default: `true`

Enable to zoom the image by dragging touch.

### zoomOnWheel

- Type: `Boolean`
- Default: `true`

Enable to zoom the image by mouse wheeling.

### wheelZoomRatio

- Type: `Number`
- Default: `0.1`

Define zoom ratio when zooming the image by mouse wheeling.

### cropBoxMovable

- Type: `Boolean`
- Default: `true`

Enable to move the crop box by dragging.

### cropBoxResizable

- Type: `Boolean`
- Default: `true`

Enable to resize the crop box by dragging.

### toggleDragModeOnDblclick

- Type: `Boolean`
- Default: `true`

Enable to toggle drag mode between `"crop"` and `"move"` when clicking twice on the cropper.

> Requires [`dblclick`](https://developer.mozilla.org/en-US/docs/Web/Events/dblclick) event support.

### minContainerWidth

- Type: `Number`
- Default: `200`

The minimum width of the container.

### minContainerHeight

- Type: `Number`
- Default: `100`

The minimum height of the container.

### minCanvasWidth

- Type: `Number`
- Default: `0`

The minimum width of the canvas (image wrapper).

### minCanvasHeight

- Type: `Number`
- Default: `0`

The minimum height of the canvas (image wrapper).

### minCropBoxWidth

- Type: `Number`
- Default: `0`

The minimum width of the crop box.

**Note:** This size is relative to the page, not the image.

### minCropBoxHeight

- Type: `Number`
- Default: `0`

The minimum height of the crop box.

**Note:** This size is relative to the page, not the image.

### ready

- Type: `Function`
- Default: `null`

A shortcut of the `ready` event.

### cropstart

- Type: `Function`
- Default: `null`

A shortcut of the `cropstart` event.

### cropmove

- Type: `Function`
- Default: `null`

A shortcut of the `cropmove` event.

### cropend

- Type: `Function`
- Default: `null`

A shortcut of the `cropend` event.

### crop

- Type: `Function`
- Default: `null`

A shortcut of the `crop` event.

### zoom

- Type: `Function`
- Default: `null`

A shortcut of the `zoom` event.

## 示例

```
<!-- 使用块元素（容器）包装图像或画布元素 -->
<div>
  <img id="image" src="picture.jpg">
</div>
```



```
/* 确保图像的大小与容器完全匹配 */
img {
  display: block;

  /* 很重要 */
  max-width: 100%;
}
```

```
const image = document.getElementById('image');
const cropper = new Cropper(image, {
  aspectRatio: 16 / 9,
  crop(event) {
    console.log(event.detail.x);
    console.log(event.detail.y);
    console.log(event.detail.width);
    console.log(event.detail.height);
    console.log(event.detail.rotate);
    console.log(event.detail.scaleX);
    console.log(event.detail.scaleY);
  },
});
```



## Methods

As there is an **asynchronous** process when loading the image, you **should call most of the methods after ready**, except `setAspectRatio`, `replace` and `destroy`.

> If a method doesn't need to return any value, it will return the cropper instance (`this`) for chain composition.

```
new Cropper(image, {
  ready() {
    // this.cropper[method](argument1, , argument2, ..., argumentN);
    this.cropper.move(1, -1);

    // Allows chain composition
    this.cropper.move(1, -1).rotate(45).scale(1, -1);
  },
});
```

### crop()

手动显示裁剪框

```
new Cropper(image, {
  autoCrop: false,
  ready() {
    // Do something here
    // ...

    // And then
    this.cropper.crop();
  },
});
```

### reset()

将图像和裁剪框重置为其初始状态。

### clear()

Clear the crop box.

### replace(url[, hasSameSize])

- **url**:
  - Type: `String`
  - A new image url.
- **hasSameSize** (optional):
  - Type: `Boolean`
  - Default: `false`
  - 如果新图像的大小与旧图像相同，则它不会重建裁剪器，而只更新所有相关图像的URL。这可用于应用过滤器。

Replace the image's src and rebuild the cropper.

### enable()

Enable (unfreeze) the cropper.

### disable()

禁用（冻结）裁剪器。

### destroy()

销毁裁剪器并从映像中删除实例。

### move(offsetX[, offsetY])

- **offsetX**:
  - Type: `Number`
  - Moving size (px) in the horizontal direction.
- **offsetY** (optional):
  - Type: `Number`
  - Moving size (px) in the vertical direction.
  - If not present, its default value is `offsetX`.

Move the canvas (image wrapper) with relative offsets.

```
cropper.move(1);
cropper.move(1, 0);
cropper.move(0, -1);
```

### moveTo(x[, y])

- **x**:
  - Type: `Number`
  - The `left` value of the canvas
- **y** (optional):
  - Type: `Number`
  - The `top` value of the canvas
  - If not present, its default value is `x`.

Move the canvas (image wrapper) to an absolute point.

### zoom(ratio)

- ratio

  :

  - Type: `Number`
  - Zoom in: requires a positive number (ratio > 0)
  - Zoom out: requires a negative number (ratio < 0)

以相对比例缩放画布（图像包装）。

```
cropper.zoom(0.1);
cropper.zoom(-0.1);
```

### zoomTo(ratio[, pivot])

- **ratio**:
  - Type: `Number`
  - Requires a positive number (ratio > 0)
- **pivot** (optional):
  - Type: `Object`
  - Schema: `{ x: Number, y: Number }`
  - The coordinate of the center point for zooming, base on the top left corner of the cropper container.

Zoom the canvas (image wrapper) to an absolute ratio.

```
cropper.zoomTo(1); // 1:1 (canvasData.width === canvasData.naturalWidth)

const containerData = cropper.getContainerData();

// Zoom to 50% from the center of the container.
cropper.zoomTo(.5, {
  x: containerData.width / 2,
  y: containerData.height / 2,
});
```

### rotate(degree)

- degree

  :

  - Type: `Number`
  - Rotate right: requires a positive number (degree > 0)
  - Rotate left: requires a negative number (degree < 0)

Rotate the image with a relative degree.

> Requires [CSS3 2D Transforms](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) support ([IE 9+](https://caniuse.com/transforms2d)).

```
cropper.rotate(90);
cropper.rotate(-90);
```

### rotateTo(degree)

- degree

  :

  - Type: `Number`

Rotate the image to an absolute degree.

### scale(scaleX[, scaleY])

- **scaleX**:
  - Type: `Number`
  - Default: `1`
  - 应用于图像横坐标的比例因子。
  - 当等于'1'时，它不起任何作用。
- **scaleY** (optional):
  - Type: `Number`
  - 要应用于图像纵坐标的比例因子。
  - 如果不存在，则其默认值为“scaleX”。

Scale the image.

> Requires [CSS3 2D Transforms](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) support ([IE 9+](https://caniuse.com/transforms2d)).

```
cropper.scale(-1); // Flip both horizontal and vertical
cropper.scale(-1, 1); // Flip horizontal
cropper.scale(1, -1); // Flip vertical
```

### scaleX(scaleX)

- scaleX

  :

  - Type: `Number`
  - Default: `1`
  - The scaling factor to apply to the abscissa of the image.
  - When equal to `1` it does nothing.

Scale the abscissa of the image.

### scaleY(scaleY)

- scaleY

  :

  - Type: `Number`
  - Default: `1`
  - The scaling factor to apply on the ordinate of the image.
  - When equal to `1` it does nothing.

Scale the ordinate of the image.

### getData([rounded])

- **rounded** (optional):
  - Type: `Boolean`
  - Default: `false`
  - Set `true` to get rounded values.
- (return value):
  - Type: `Object`
  - Properties:
    - `x`: the offset left of the cropped area
    - `y`: the offset top of the cropped area
    - `width`: the width of the cropped area
    - `height`: the height of the cropped area
    - `rotate`: the rotated degrees of the image
    - `scaleX`: the scaling factor to apply on the abscissa of the image
    - `scaleY`: the scaling factor to apply on the ordinate of the image

Output the final cropped area position and size data (base on the natural size of the original image).

> You can send the data to the server-side to crop the image directly:
>
> 1. Rotate the image with the `rotate` property.
> 2. Scale the image with the `scaleX` and `scaleY` properties.
> 3. Crop the image with the `x`, `y`, `width`, and `height` properties.

[![A schematic diagram for data's properties](https://github.com/fengyuanchen/cropperjs/raw/main/docs/images/data.jpg)](https://github.com/fengyuanchen/cropperjs/blob/main/docs/images/data.jpg)

### setData(data)

- data

  :

  - Type: `Object`
  - Properties: See the [`getData`](https://github.com/fengyuanchen/cropperjs#getdatarounded) method.
  - You may need to round the data properties before passing them in.

Change the cropped area position and size with new data (base on the original image).

> **Note:** This method only available when the value of the `viewMode` option is greater than or equal to `1`.

### getContainerData()

- (return value):
  - Type: `Object`
  - Properties:
    - `width`: the current width of the container
    - `height`: the current height of the container

Output the container size data.

[![A schematic diagram for cropper's layers](https://github.com/fengyuanchen/cropperjs/raw/main/docs/images/layers.jpg)](https://github.com/fengyuanchen/cropperjs/blob/main/docs/images/layers.jpg)

### getImageData()

- (return value):
  - Type: `Object`
  - Properties:
    - `left`: the offset left of the image
    - `top`: the offset top of the image
    - `width`: the width of the image
    - `height`: the height of the image
    - `naturalWidth`: the natural width of the image
    - `naturalHeight`: the natural height of the image
    - `aspectRatio`: the aspect ratio of the image
    - `rotate`: the rotated degrees of the image if it is rotated
    - `scaleX`: the scaling factor to apply on the abscissa of the image if scaled
    - `scaleY`: the scaling factor to apply on the ordinate of the image if scaled

Output the image position, size, and other related data.

### getCanvasData()

- (return value):
  - Type: `Object`
  - Properties:
    - `left`: the offset left of the canvas
    - `top`: the offset top of the canvas
    - `width`: the width of the canvas
    - `height`: the height of the canvas
    - `naturalWidth`: the natural width of the canvas (read only)
    - `naturalHeight`: the natural height of the canvas (read only)

Output the canvas (image wrapper) position and size data.

```
const imageData = cropper.getImageData();
const canvasData = cropper.getCanvasData();

if (imageData.rotate % 180 === 0) {
  console.log(canvasData.naturalWidth === imageData.naturalWidth);
  // > true
}
```

### setCanvasData(data)

- data

  :

  - Type: `Object`
  - Properties:
    - `left`: the new offset left of the canvas
    - `top`: the new offset top of the canvas
    - `width`: the new width of the canvas
    - `height`: the new height of the canvas

Change the canvas (image wrapper) position and size with new data.

### getCropBoxData()

- (return value):
  - Type: `Object`
  - Properties:
    - `left`: the offset left of the crop box
    - `top`: the offset top of the crop box
    - `width`: the width of the crop box
    - `height`: the height of the crop box

Output the crop box position and size data.

### setCropBoxData(data)

- data

  :

  - Type: `Object`
  - Properties:
    - `left`: the new offset left of the crop box
    - `top`: the new offset top of the crop box
    - `width`: the new width of the crop box
    - `height`: the new height of the crop box

Change the crop box position and size with new data.

### getCroppedCanvas([options])

- **options** (optional):
  - Type: `Object`
  - Properties:
    - `width`: the destination width of the output canvas.
    - `height`: the destination height of the output canvas.
    - `minWidth`: the minimum destination width of the output canvas, the default value is `0`.
    - `minHeight`: the minimum destination height of the output canvas, the default value is `0`.
    - `maxWidth`: the maximum destination width of the output canvas, the default value is `Infinity`.
    - `maxHeight`: the maximum destination height of the output canvas, the default value is `Infinity`.
    - `fillColor`: a color to fill any alpha values in the output canvas, the default value is the `transparent`.
    - [`imageSmoothingEnabled`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/imageSmoothingEnabled): set to change if images are smoothed (`true`, default) or not (`false`).
    - [`imageSmoothingQuality`](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/imageSmoothingQuality): set the quality of image smoothing, one of "low" (default), "medium", or "high".
- (return value):
  - Type: `HTMLCanvasElement`
  - A canvas drawn the cropped image.
- Notes:
  - The aspect ratio of the output canvas will be fitted to the aspect ratio of the crop box automatically.
  - If you intend to get a JPEG image from the output canvas, you should set the `fillColor` option first, if not, the transparent part in the JPEG image will become black by default.
  - Uses the Browser's native [canvas.toBlob](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toBlob) API to do the compression work, which means it is **lossy compression**. For better image quality, you can upload the original image and the cropped data to a server and do the crop work on the server.
- Browser support:
  - Basic image: requires [Canvas](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement) support ([IE 9+](https://caniuse.com/canvas)).
  - Rotated image: requires [CSS3 2D Transforms](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) support ([IE 9+](https://caniuse.com/transforms2d)).
  - Cross-origin image: requires HTML5 [CORS settings attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_settings_attributes) support ([IE 11+](https://caniuse.com/cors)).

Get a canvas drawn from the cropped image (lossy compression). If it is not cropped, then returns a canvas drawn the whole image.

> After then, you can display the canvas as an image directly, or use [HTMLCanvasElement.toDataURL](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toDataURL) to get a Data URL, or use [HTMLCanvasElement.toBlob](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toBlob) to get a blob and upload it to server with [FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData) if the browser supports these APIs.

Avoid get a blank (or black) output image, you might need to set the `maxWidth` and `maxHeight` properties to limited numbers, because of [the size limits of a canvas element](https://stackoverflow.com/questions/6081483/maximum-size-of-a-canvas-element). Also, you should limit the maximum zoom ratio (in the `zoom` event) for the same reason.

```
cropper.getCroppedCanvas();

cropper.getCroppedCanvas({
  width: 160,
  height: 90,
  minWidth: 256,
  minHeight: 256,
  maxWidth: 4096,
  maxHeight: 4096,
  fillColor: '#fff',
  imageSmoothingEnabled: false,
  imageSmoothingQuality: 'high',
});

// Upload cropped image to server if the browser supports `HTMLCanvasElement.toBlob`.
// The default value for the second parameter of `toBlob` is 'image/png', change it if necessary.
cropper.getCroppedCanvas().toBlob((blob) => {
  const formData = new FormData();

  // Pass the image file name as the third parameter if necessary.
  formData.append('croppedImage', blob/*, 'example.png' */);

  // Use `jQuery.ajax` method for example
  $.ajax('/path/to/upload', {
    method: 'POST',
    data: formData,
    processData: false,
    contentType: false,
    success() {
      console.log('Upload success');
    },
    error() {
      console.log('Upload error');
    },
  });
}/*, 'image/png' */);
```

### setAspectRatio(aspectRatio)

- aspectRatio

  :

  - Type: `Number`
  - Requires a positive number.

Change the aspect ratio of the crop box.

### setDragMode([mode])

- mode

   

  (optional):

  - Type: `String`
  - Default: `'none'`
  - Options: `'none'`, `'crop'`, `'move'`

Change the drag mode.

**Tips:** You can toggle the "crop" and "move" mode by double click on the cropper.

[⬆ back to top](https://github.com/fengyuanchen/cropperjs#table-of-contents)

## Events

### ready

This event fires when the target image has been loaded and the cropper instance is ready for operating.

```
let cropper;

image.addEventListener('ready', function () {
  console.log(this.cropper === cropper);
  // > true
});

cropper = new Cropper(image);
```

### cropstart

- **event.detail.originalEvent**:
  - Type: `Event`
  - Options: `pointerdown`, `touchstart`, and `mousedown`
- **event.detail.action**:
  - Type: `String`
  - Options:
    - `'crop'`: create a new crop box
    - `'move'`: move the canvas (image wrapper)
    - `'zoom'`: zoom in / out the canvas (image wrapper) by touch.
    - `'e'`: resize the east side of the crop box
    - `'w'`: resize the west side of the crop box
    - `'s'`: resize the south side of the crop box
    - `'n'`: resize the north side of the crop box
    - `'se'`: resize the southeast side of the crop box
    - `'sw'`: resize the southwest side of the crop box
    - `'ne'`: resize the northeast side of the crop box
    - `'nw'`: resize the northwest side of the crop box
    - `'all'`: move the crop box (all directions)

This event fires when the canvas (image wrapper) or the crop box starts to change.

```
image.addEventListener('cropstart', (event) => {
  console.log(event.detail.originalEvent);
  console.log(event.detail.action);
});
```

### cropmove

- **event.detail.originalEvent**:
  - Type: `Event`
  - Options: `pointermove`, `touchmove`, and `mousemove`.
- **event.detail.action**: the same as "cropstart".

This event fires when the canvas (image wrapper) or the crop box is changing.

### cropend

- **event.detail.originalEvent**:
  - Type: `Event`
  - Options: `pointerup`, `pointercancel`, `touchend`, `touchcancel`, and `mouseup`.
- **event.detail.action**: the same as "cropstart".

This event fires when the canvas (image wrapper) or the crop box stops to change.

### crop

- **event.detail.x**
- **event.detail.y**
- **event.detail.width**
- **event.detail.height**
- **event.detail.rotate**
- **event.detail.scaleX**
- **event.detail.scaleY**

> About these properties, see the [`getData`](https://github.com/fengyuanchen/cropperjs#getdatarounded) method.

This event fires when the canvas (image wrapper) or the crop box changed.

**Notes:**

- When the `autoCrop` option is set to the `true`, a `crop` event will be triggered before the `ready` event.
- When the `data` option is set, another `crop` event will be triggered before the `ready` event.

### zoom

- **event.detail.originalEvent**:
  - Type: `Event`
  - Options: `wheel`, `pointermove`, `touchmove`, and `mousemove`.
- **event.detail.oldRatio**:
  - Type: `Number`
  - The old (current) ratio of the canvas
- **event.detail.ratio**:
  - Type: `Number`
  - The new (next) ratio of the canvas (`canvasData.width / canvasData.naturalWidth`)

This event fires when a cropper instance starts to zoom in or zoom out its canvas (image wrapper).

```
image.addEventListener('zoom', (event) => {
  // Zoom in
  if (event.detail.ratio > event.detail.oldRatio) {
    event.preventDefault(); // Prevent zoom in
  }

  // Zoom out
  // ...
});
```
