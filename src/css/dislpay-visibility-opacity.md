# dislpay、visibility、opacity 的区别？

### 共同点

都可以显示和隐藏元素。

### 不同点

1. display: none（灰飞烟灭）
    1. DOM 结构：浏览器不会渲染 display 属性为 none 的元素，**不占据空间**；
    2. 事件监听：**无法进行 DOM 事件监听**；
    3. 性能：动态改变此属性时会引起**重排和重绘**，性能较差；
    4. 继承：**不会被子元素继承**，毕竟子类也不会被渲染；
    5. transition：**transition 不支持 display**。

2. visibility: hidden（灵魂出窍）
    1. DOM 结构：元素被隐藏，但是会被渲染不会消失，**占据空间**；
    2. 事件监听：**无法进行 DOM 事件监听**；
    3. 性 能：动态改变此属性时只会引起**重绘**，性能较高；
    4. 继 承：会被子元素继承，**子元素可以通过设置 visibility: visible; 来取消隐藏**；
    5. transition：**transition 支持 visibility。**，visibility 会立即显示，隐藏时会延时(但无动画效果)。

3. opacity: 0（醉生梦死）
    1. DOM 结构：透明度为 100%，元素隐藏，**占据空间**；
    2. 事件监听：**可以进行 DOM 事件监听**；
    3. 性 能：提升为合成层，**不会触发重排和重绘**，性能较高；
    4. 继 承：**会被子元素继承,且，子元素并不能通过 opacity: 1 来取消隐藏**；
    5. transition：**transition 支持 opacity。**