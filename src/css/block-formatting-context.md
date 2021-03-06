# 块级格式化上下文 BFC

Block Formatting Context 中文称为 **块级格式化上下文/块格式化上下文**，但是作为前端你应该知道 CSS 中不止一种 FC，还有 IFC、GFC、FFC，其中 BFC 和 IFC 是属于 CSS2.1 的内容， GFC 和 FFC 是属于 CSS3 的内容，一起来学习下，先从 FC 说起。

### 什么是 FC？

FC（Formatting Context）格式化上下文：

它是 W3C CSS2.1 规范中的一个概念，定义的是**页面中的一块渲染区域**，并且有**一套渲染规则**，它决定了其**子元素将如何定位**，以及和其他元素的**关系和相互作用**。

简单来讲 FC 就是定义了页面盒模型的渲染规则(或叫布局规则)。


说到布局规则就不得不提**标准文档流**下，block 元素 和 line 元素的布局规则：

1. 内联盒子（正常渲染规则从左到右）
2. 块级盒子（正常渲染规则从上到下）

一个元素到底属于什么类型的盒模型是由 display 这个属性决定的，不同类型的 Box，会参与不同的 Formatting Context：

- Block level 的 box 会参与形成 BFC，比如 display 值为 block，list-item，table 的元素。
- Inline level 的 box 会参与形成 IFC，比如 display 值为 inline，inline-table，inline-block 的元素。

接下来思考一个 very important matter:

> 请问一个 block 元素默认是 BFC 吗？why?

这个问题我思考了好久，可以明确靠诉你，**block 元素默认不是 BFC**，原因是 MDN 上讲 BFC 的创建，其中有一条是：

> Block elements where overflow has a value other than visible and clip.（块级元素的overflow的值不能为 visible/clip时才会创建 BFC）

**我们知道块级元素的 overflow 值默认为 visible，所以块级元素不是 BFC。**，举一反三我们也应该知道 html 标签是 BFC，因为 html 是块级元素同时 overflow 的默认值为 auto，但是 textarea 标签不是 BFC ，虽然它的 overflow 的默认值为 auto，但是它属于内联元素。


### BFC 

常用的创建 BFC 的条件有：

1. float 的值为 left/right 或不为 none。
2. position 的值为 absolute/fixed ，或不为 static 和 relative；
3. display 的值为 inline-block/table-cell/table-caption
4. overflow 的值为 auto/scroll/hidden 或不为 visible
5. 根元素(html)

更加完正的创建 BFC 的条件有：[MDN create BFC](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context)

关于：

1. 弹性元素（display 为 flex 或 inline-flex 元素的直接子元素）
2. 网格元素（display 为 grid 或 inline-grid 元素的直接子元素）
3. 多列容器（元素的 column-count 或 column-width 不为 auto，包括 column-count 为 1）

这三个布局相关的盒子，到底属不属于 BFC 呢，MDN 写的是属于的，姑且可以认为 GFC 和 FFC 属于 BFC 的子集。

BFC 的约束规则：

1. 内部的盒子会在垂直方向上一个接一个的放置；
2. 垂直方向上的距离由 margin 决定。（完整的说法是：属于同一个 BFC<的高度时，浮动子元素也参与计算； 的俩个相邻的 BOX 的 margin 会发生重叠，与方向无关。）
3. 每个元素的左外边距与包含块的左边界相接触（从左到右），即使浮动元素也是如此。（这说明BFC中的子元素不会超出它的包含块，而position 为 absolute 的元素可以超出它的包含块边界）；
4. **BFC 的区域不会与 float 的元素区域重叠；**
5. **计算 BFC 的高度时，浮动子元素也参与计算；**
6. **BFC 就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然；**

BFC 的应用：

1. 防止 margin 发生重叠，两端对齐。（BFC 的封闭性）
2. 防止发生因浮动导致的**高度塌陷**（计算 BFC 的高度时，浮动子元素也参与计算）。
3. 两栏自适应布局（BFC 的区域不会与 float 的元素区域重叠）。

此处要讲下两栏自适应布局的历史，不过首先有请重磅人物出场：overflow。

### 两栏自适应布局 和 overflow 介绍

overflow 的值可以为：

1. visible（一般盒子的默认值，元素超出盒子依然坚挺的显示）
2. hidden（溢出隐藏）
3. scroll（不管溢不溢出，滚动条常在）
4. auto（溢出时，自动出现滚动条）
5. inherit（继承，不用多讲）

关于 overflow-x 和 overflow-y ：

- 如果 overflow-x 和 overflow-y 值相同，就等于 overflow
- 如果 overflow-x 和 overflow-y 值不相同时，且有一个属性值为 visible ，另一个属性值设置除 visible 以外的（auto/hidden/scroll）值，则为属性值 visible 的会被强制重置为 auto。

关于浏览器滚动条：

出现滚动条的条件是 `overflow: srcoll/auto;`，其中 **html 和 textarea 标签 overflow 默认为 auto，其他元素的 overflow 则为 visible。**

> 问：默认浏览器的滚动条是在 html 标签上还是 body 标签上？
> 答：记住无论什么浏览器，默认的滚动条都来自 **html 标签，而不是 body 标签**。如何证明：
> 默认情况下 body 标签有 8 像素的 margin，如果滚动条来自 body，因为滚动条属于宽度的一部分，滚动条距离浏览器边框应该是有 8px 的距离，真实观察未产生距离，所以滚动条来自 html 元素无疑。

经典问题，如何解决水平居中出现滚动条造成的跳动的问题：

1. overflow-y: scroll;
2. padding-left: calc(100vw - 100%);

接下来重头戏，两栏自适应布局，我们一般来讲是**通过 position 和 float 来做**，相对 position 来讲 float 更加较为常用。我们就以 float 为演示，然后又分为两类：

1. 流体自适应布局

这个简单易懂了：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>两栏自适应布局</title>
    <style>
        * { margin: 0; padding: 0; }
        section {
            height: 100vh;
        }
        aside {
            float: left;
            width: 230px;
            height: 10em;
            background-color: aqua;
        }
        main {
            margin-left: 250px;
            height: 10em;
            background-color: skyblue;
        }
    </style>
</head>
<body>
    <section>
        <aside></aside>
        <main>
            大家好
        </main>
    </section>
</body>
</html>
```

![流体两栏自适应布局.png](./img/block-formatting-context/流体两栏自适应布局.png)

当我们给 main 标签添加 `clear: both;` 清除浮动：

```css
main::before {
    content: "";
    display: block;
    clear: both;
}
```

内容折行了：
![clear-both](./img/block-formatting-context/clear-both.png)

流体自适应布局的缺点：
    1. 设置 `clear: both;` 会折行显示
    2. 两栏之间的间隙，需要根据左侧的一栏宽度自己计算。

2. BFC 两栏自适应布局

BFC 两栏自适应布局，可以避免 `clear: both` 的影响，利用的是 BFC 的区域不会与 float 的元素区域重叠。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BFC 两栏自适应布局</title>
    <style>
        * { margin: 0; padding: 0; }
        section {
            height: 100vh;
        }
        aside {
            float: left;
            width: 230px;
            height: 10em;
            background-color: aqua;
        }
        main {
            overflow: hidden;
            height: 10em;
            background-color: skyblue;
        }
        main::before {
            content: "";
            display: block;
            clear: both;
        }
    </style>
</head>
<body>
    <section>
        <aside></aside>
        <main>
            大家好
        </main>
    </section>
</body>
</html>
```
![BFC-造成贴边.png](./img/block-formatting-context/BFC-造成贴边.png)

这时候要注意设置 `overflow: hidden;` 的盒子的 margin-left 还是从浏览器的左边开始计算，所以要想两栏间距为 20px，不是设置 `margin-left: 20px;` 而是 `margin-left: 250px;`。比较麻烦所以可使用 padding-left 替代。但是无论 margin 还是 padding 一旦左侧栏目宽度变更，要想保持两栏间隔 20px，需要重新计算。**所以最好的做法是左侧浮动的盒子设置**`margin-right: 20px;` 这样无论左右盒子如何宽度如何变化，两个盒子之前的宽度永远都是 20px，真正做到了自适应。

通过 BFC 我们做到了**真正的布局自适应**，同时避免了 `clear: both` 折行的影响，**但是有一个小小的瑕疵，`overflow: hidden;` 导致溢出隐藏**。而浏览器大多数盒子的 overflow 属性默认值为 visible。接下来我们改变创建 BFC 的方式，来要克服这个问题，使用 `display:table-cell` 来创建 BFC ，原因是：

> `display:table-cell` 让元素表现得像单元格一样，跟 `display:inline-block` 一样，会跟随内部元素的宽度显示，像单元格一样有个非常神奇的特性，就是宽度值设置的再大，实际宽度也不会超过表格容器的宽度。

**两栏自适应布局终极解决方案：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>最终极版两栏自适应布局</title>
    <style>
        * { margin: 0; padding: 0; }
        section {
            height: 100vh;
        }
        aside {
            float: left;
            width: 230px;
            height: 10em;
            margin-right: 20px;
            background-color: aqua;
        }
        main {
            display: table-cell;
            height: 10em;
            width: 100vw;
            background-color: skyblue;
        }
        main::before {
            content: "";
            display: block;
            clear: both;
        }
    </style>
</head>
<body>
    <section>
        <aside></aside>
        <main>
            大家好
        </main>
    </section>
</body>
</html>
```

**注意⚠️：**[columns 分栏布局/多栏布局](https://www.zhangxinxu.com/wordpress/2019/01/css-css3-columns-layout/#columns) 也属于 BFC。

参考链接：张鑫旭的 [CSS深入理解流体特性和BFC特性下多栏自适应布局](https://www.zhangxinxu.com/wordpress/2015/02/css-deep-understand-flow-bfc-column-two-auto-layout/)

设置 `display: table-cell;` 之后，宽度现在最好给值大于 100vw 就行了，没必要 9999px 这样。

> 行文至此，接下去研究 IFC/FFC/GFC 的时候遇见瓶颈了，有些概念术语不是很懂，所以跑去补充下 CSS 中的术语 [一些搞不清楚的 CSS 术语](./some-of-css-terms.md)

### IFC

IFC(Inline Formatting Context) 直译为 **行内/内联格式化上下文**，IFC 的 line box （线框）高度由其包含行内元素中最高的实际高度计算而来（不受到竖直方向的 padding/margin 影响)。

> BFC 应用价值比较高，面试也常问，所以大家比较了解，但是你有没有想过生成 BFC 需要一定的条件，那么生成 IFC 是否需要呢？或者说如何生成 IFC？

我翻便 W3C [inline-formatting](https://www.w3.org/TR/CSS2/visuren.html#inline-formatting) 和 MDN [Inline_formatting_context](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Inline_formatting_context) 都没看到具体写如何生成 IFC 的方法，先看 IFC 的特性，我们再来看这个问题。

#### IFC 特性

W3C [inline-formatting](https://www.w3.org/TR/CSS2/visuren.html#inline-formatting) 对 IFC 的描述如下：

> In an inline formatting context, boxes are laid out horizontally, one after the other, beginning at the top of a containing block. Horizontal margins, borders, and padding are respected between these boxes. The boxes may be aligned vertically in different ways: their bottoms or tops may be aligned, or the baselines of text within them may be aligned.

翻译一下 IFC 有三个特点：

1. 在行内格式化上下文中，框(boxes)一个接一个地水平排列，起点是包含块的顶部。
2. 盒子水平方向上的 margin，border 和 padding 有效。
3. 盒子在垂直方向上可以以不同的方式对齐：它们的顶部或底部对齐，或根据其中文字的基线对齐。

除了上面 IFC 还有以下特点：

- 能把同在一行上的框都完全包含进去的一个矩形区域，被称为该行的**行框（line box）**。
- 行框的宽度是由包含块（containing box）和与其中的浮动来决定。
- IFC 中的 line box 一般左右边贴紧其包含块，但 float 元素会优先排列，即浮动元素会改变 line box  的宽度。
- IFC 中的 line box 高度由 CSS 行高计算规则来确定，同一个 IFC 下的多个 line box 高度可能会不同。
- 当 inline-level boxes 的总宽度少于包含它们的 line box 时，其水平渲染规则由 text-align 属性值来决定。
- 当一个 inline box 超过父元素的宽度时，它会被分割成多个 boxes，这些 boxes 分布在多个 line box 中。如果子元素未设置强制换行的情况下，inline box 将不可被分割，将会溢出父元素。

#### IFC 的应用

图文跟随：结合浮动元素做图文跟随。
水平居中：当一个块要在环境中水平居中时，设置其为 inline-block 则会在外层产生 IFC ，通过 text-align 则可以使其水平居中。
垂直居中：创建一个 IFC ，用其中一个元素撑开父元素的高度，然后设置其 vertical-align:middle ，其他行内元素则可以在此父元素下垂直居中。

### FFC

FFC(Flex Formatting Contexts) 直译为 **自适应格式化上下**。

display 值为 flex 或者 inline-flex 会形成 FFC，只需要注意，设为 Flex 布局以后，子元素的 float、clear 和vertical-align 属性将失效。

Flex 现在是应用最广的一种布局了，我认为阮老师的 [Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html) 写的是最好的。

### GFC

GFC(GridLayout Formatting Contexts) 直译为 **网格布局格式化上下文**。

当为一个元素设置 display 值为 grid 的时候，会形成 GFC，需要注意的是，设为网格布局以后，容器子元素（项目）的float、display: inline-block、display: table-cell、vertical-align 和 column-* 等设置都将失效。。

也是阮老师的文章 [CSS Grid 网格布局教程](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html) 工作中我实际用到了一次，体验不是很好，主要它是属性相比 flex 的属性太多难记，Grid 教程看好几次了，现在还是一脸蒙 B。
