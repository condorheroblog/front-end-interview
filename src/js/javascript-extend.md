# JavaScript 常见的六种继承方式

### 原型链继承(prototype)

Child.prototype = new Parent();

### 使用 call/apply 在构造函数里继承

```
// Parent 不再通过 new 触发
function Parent () {
    this.names = ['kevin', 'daisy'];
}

function Child () {
    Parent.call(this);
}
```
### extends
### Object.assign
https://github.com/mqyqingfeng/Blog/issues/16
https://github.com/ljianshu/Blog/issues/20