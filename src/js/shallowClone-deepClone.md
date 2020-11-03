# 深拷贝(deep copy)和浅拷贝(shallow copy)


### 浅拷贝（shallowColone）

1. Object.assign()
2. 展开运算符...
3. Array.prototype.concat()
4. Array.prototype.slice()


### 深拷贝

1. JSON.parse(JSON.stringify())

这种方法虽然可以实现数组或对象深拷贝,但不能处理函数和正则，因为这两者基于 JSON.stringify 和 JSON.parse 处理后，得到的正则就不再是正则（变为空对象），得到的函数就不再是函数（变为null）了。

值为 undefined 的属性在转换后丢失；
值为 Symbol 类型的属性在转换后丢失；
值为 RegExp 对象的属性在转换后变成了空对象；
值为 函数对象的属性在转换后丢失；
值为 Date 对象的属性在转换后变成了字符串；
会抛弃对象的 constructor,所有的构造函数会指向Object；

对象的循环引用会抛出错误。
2. 手写递归方法

```js
function deepClone(obj, hash = new WeakMap()) {
  if (obj === null) return obj; // 如果是null或者undefined我就不进行拷贝操作
  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof RegExp) return new RegExp(obj);
  // 可能是对象或者普通的值  如果是函数的话是不需要深拷贝
  if (typeof obj !== "object") return obj;
  // 是对象的话就要进行深拷贝
  if (hash.get(obj)) return hash.get(obj);
  let cloneObj = new obj.constructor();
  // 找到的是所属类原型上的constructor,而原型上的 constructor指向的是当前类本身
  hash.set(obj, cloneObj);
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      // 实现一个递归拷贝
      cloneObj[key] = deepClone(obj[key], hash);
    }
  }
  return cloneObj;
}
let obj = { name: 1, address: { x: 100 } };
obj.o = obj; // 对象存在循环引用的情况
let d = deepClone(obj);
obj.address.x = 200;
console.log(d);
```