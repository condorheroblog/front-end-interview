# margin-collapsing

> margin-collapsing 我们中文一般称为 「margin 塌陷」或叫 「margin 重叠」。

### 可视尺寸 与 占据尺寸

- 可视尺寸即肉眼能看到的尺寸等于 padding + border + width
- 占据尺寸即能占据空间的尺寸等于 margin + padding + border + width

观察上面可知：占据尺寸 = 可视尺寸 + margin。

### margin 如何影响可视尺寸与占据尺寸

1. 对可视尺寸来讲（只影响可视尺寸的width）
    - 垂直方向上永远影响不了
    - 块级元素水平方向未设置宽度时，margin 为正，可视尺寸缩小，margin 为负，可视尺寸扩大。

我曾今遇到有的人给盒子扩展 1px 这么写过：`width： calc(100% + 1px)`，完全没必要直接 `margin-right/margin-left: -1px;` 即可解决。

2. 对占据尺寸来讲（只影响占据尺寸的margin）
    - 对 block 元素来讲影响任何方向
    - 对 inline 元素只影响水平方向


### margin 百分比单位参考谁

### margin 重叠发生的条件

1. block element（块级元素）
2. 只会发现在垂直方向上，也就是只有 margin-top margin-bottom 两个属性会出现， margin-right margin-left 则不会出现。

### margin 重叠常见场景

1. 相邻的兄弟元素

```html
div {
    width: 200px;
    height: 200px;
}
.brother-1 {
    background-color: #0099cc;
    margin-bottom: 50px;
}
.brother-2 {
    background-color: #cc33cc;
    margin-top: 150px;
}
<div class="brother-1"></div>
<div class="brother-2"></div>
```

2. 父级和第一个/最后一个子元素（父级只有一个元素）

以下三种情况等价：
```html
// 第一种
.father {
    width: 200px;
    height: 200px;
    background-color: #0099cc;
}
.son {
     width: 100px;
    height: 100px;
    background-color: #cc33cc;
    margin-top: 150px;
}
// 第二种
.father {
    width: 200px;
    height: 200px;
    background-color: #0099cc;
    margin-top: 150px;
}
.son {
     width: 100px;
    height: 100px;
    background-color: #cc33cc;
}
// 第三种
.father {
    width: 200px;
    height: 200px;
    background-color: #0099cc;
    margin-top: 150px;
}
.son {
    width: 100px;
    height: 100px;
    background-color: #cc33cc;
    margin-top: 150px;
}
<div class="father">
    <div class="son"></div>
</div>
```
3. 空的块级元素，自我重叠

```html
*{ margin: 0; }
div {
    width: 200px;
    height: 200px;
}
.A {
    background-color: #0099cc;
}
.B {
    background-color: #cc33cc;
}
.empty {
    margin-top: 10px;
    margin-bottom: 10px;
}
<div class="A"></div>
<p class="empty"></p>
<div class="B"></div>
```
### 解决 margin 重叠的方法

#### 解决父子 margin 重叠的方法
1. 采用 -webkit-margin-collapse: separate; 去除
2. 块级格式化上下文
3. 对 margin-top 设置 padding-top，对 margin-bottom 设置 padding-bottom
4. 父子之前没有行内元素，例如加个 `&nbsp;` 空格利用伪元素给子元素的前面添加一个空元素
5. 只针对 margin-bottom 父元素设置 height/min-height/max-height

#### 解决空块级元素 margin 重叠的方法

1. 空元素没有设置 border
2. 空元素没有设置 padding
3. 空元素里面没有 inline 元素
4. 空元素不能设置 height/min-height，不过可以设置 max-height

#### 解决相邻元素 margin 重叠的方法

1. 包裹一层父元素，创建 BFC
### 去除 margin 塌陷

我们可以使用 -webkit-margin-collapse 及其分拆后的 -webkit-margin-top-collapse、-webkit-margin-bottom-collapse 等属性来控制 margin 塌陷的现象。

- collapse 默认值发生重叠；

- discard 取消重叠，margin 重叠部分为 0 即无 margin；

- separate 不发生重叠，margin 重叠部分为 margin-top + margin-bottom；

### margin 合并取值方法
1. 正正取大
2. 负负取小
3. 正负取和

### margin 重叠的意义

### 在没有设置宽高的块级元素中 margin 可以影响元素的宽度。应用：一侧定宽的两栏的布局
### margin 改变元素的**占据尺寸**：等高布局 margin-bottom: -100%;padding-bottom: 100%;
### margin 的百分比
1. 普通元素相对父元素的宽。
2. 绝对定位的元素，相对最近一个定位的父元素的宽。
宽高 2:1 自适应的矩形 margin: 50%; 父overflow: hidden; 利用了 margin-collapse
宽高 1:1 自适应的矩形 margin: 100%; 父overflow: hidden;利用了 margin-collapse
width: 20%;
padding-top: 10%;

