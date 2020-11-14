# resize

指定一个 div 元素，允许用户调整大小：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>可拖拽元素</title>
</head>
<body>
    <div style="width: 200px;height: 200px;background-color: aqua; resize: both;overflow: hidden;"></div>
    <div style="width: 200px;height: 200px;background-color: aqua; resize: both;overflow: auto;"></div>
    <div style="width: 200px;height: 200px;background-color: aqua; resize: both;overflow: sceoll;"></div>
    <div style="width: 200px;height: 200px;background-color: aqua; resize: both;overflow: visible;"></div>
    <div style="width: 200px;height: 200px;background-color: aqua; resize: both;"></div>
</body>
</html>
```

resize：
|值|描述|
|:---:|:---:|
|none	|    用户无法调整元素的尺寸。|
|both	    |用户可调整元素的高度和宽度。|
|horizontal|用户可调整元素的宽度。|
|vertical	|用户可调整元素的高度。|

resize 属性指定一个元素（不能是内联元素且overflow不能为 visible）是否可以让用户手动调整大小，overflow 的值必须不是 visible。

textarea 为啥默认外挂 resize，因为它的 overflow 为 auto。