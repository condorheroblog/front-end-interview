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

这个问题我思考了，好久可以明确靠诉你不是，原因是 MDN 上讲 BFC 的创建，其中有一条是：

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

这个布局盒子，到底属不属于 BFC 呢，MDN 写的是属于的。

### IFC

IFC(Inline Formatting Context)直译为"行内格式化上下文"，IFC 的 line box （线框）高度由其包含行内元素中最高的实际高度计算而来（不受到竖直方向的  padding/margin 影响)

#### IFC有的特性

IFC 中的 line box一般左右都贴紧整个 IFC</code>，但是会因为<code>float</code>元素而扰乱。<code>float</code>元素会位于<code>IFC</code>与与<code>line box</code>之间，使得<code>line box</code>宽度缩短。</li>
<li>
<code>IFC</code>中时不可能有块级元素的，当插入块级元素时（如<code>p</code>中插入<code>div</code>）会产生两个匿名块与<code>div</code>分隔开，即产生两个<code>IFC</code>，每个<code>IFC</code>对外表现为块级元素，与<code>div</code>垂直排列。</li>

#### IFC的应用

<li>水平居中：当一个块要在环境中水平居中时，设置其为<code>inline-block</code>则会在外层产生<code>IFC</code>，通过<code>text-align</code>则可以使其水平居中。</li>
<li>垂直居中：创建一个<code>IFC</code>，用其中一个元素撑开父元素的高度，然后设置其<code>vertical-align:middle</code>，其他行内元素则可以在此父元素下垂直居中。</li>

### FFC

FFC(Flex Formatting Contexts)直译为"自适应格式化上下文"，display值为flex或者inline-flex的元素将会生成自适应容器（flex container）

### GFC

GFC(GridLayout Formatting Contexts)直译为"网格布局格式化上下文"，当为一个元素设置display值为grid的时候，此元素将会获得一个独立的渲染区域，我们可以通过在网格容器（grid container）上定义网格定义行（grid definition rows）和网格定义列（grid definition columns）属性各在网格项目（grid item）上定义网格行（grid row）和网格列（grid columns）为每一个网格项目（grid item）定义位置和空间。
那么GFC有什么用呢，和table又有什么区别呢？首先同样是一个二维的表格，但GridLayout会有更加丰富的属性来控制行列，控制对齐以及更为精细的渲染语义和控制。

一维布局变成了二维布局

### 解决的问题

1. 浮动元素高度塌陷
2. margin collapse
3. 两栏布局


### overflow 的基本属性

1. visible
2. hidden
3. scroll
4. auto
5. inherit

### 关于 overflow-x 和 overflow-y 

- 如果 overflow-x 和 overflow-y 值相同，就等于 overflow
- 如果 overflow-x 和 overflow-y 值不同，且有一个值为 visible ，另设置除 visible 以外的（auto/hidden/scroll）值，则 visible 强制重置为 auto。

### 滚动条

出现滚动条的条件：

- overflow: srcoll/auto;

html 和 textarea 标签 overflow 默认为 auto，其他元素的 overflow 则为 visible。

无论什么浏览器，默认的滚动条都来自 html 标签，而不是 body 标签。如何证明：

默认情况下 body 标签有8像素的 margin，如果滚动条来自 body，因为滚动条属于宽度的一部分，滚动条距离浏览器边框应该是有 8px 的距离，真实观察未产生距离，所以滚动条来自 html 元素无疑。

滚动条水平居中跳动的问题：

1. overflow-y: scroll;
2. padding-left: calc(100vw - 100%);


两栏自适应布局分为两种：

1. 流体自适应布局（缺点：设置clear: both;会折行显示）
2. BFC 自适应布局(不受clear: both;影响）,两栏会造成贴边，margin-left 从浏览器的左边开始计算。可使用 padding-left 替代。最好200vw;使用 .left 的 margin-right。缺点： overflow: hidden; 创建的 BFC 会溢出隐藏，使用 `display: table-cell;width:  替代`


两栏自适应布局终极解决方案：




