# cookieStore

前端唯一一个操作 Cookie 的 API 是 document.cookie（字符串的方式处理 cookie，难用至极），读写完全使用这一个 API 对我们来讲比较麻烦，所以有人进行了封装，这就是js-cookie。 现在只要你使用 Chrome 87 版以上的浏览器，都会有一个新的 API ：cookieStore（原生的 js-cookie）

通过 document.cookie 来读取某网站的 cookie 内容如下：

```js
> document.cookie
< "rec_id=edfc2bce0c9ad01d73b99f66f5bb1c42; Hm_lvt_52ee35894430604d0d3ce3b9c089adf9=1604395691; Hm_lpvt_52ee35894430604d0d3ce3b9c089adf9=1604395691; _ga=GA1.2.662417468.1604395692; _gid=GA1.2.1643174059.1604395692; _gat=1"
```

浏览器打印出 cookieStore ：

```js
> CookieStore
< CookieStore {onchange: null}
    onchange: null
    __proto__: CookieStore
        delete: ƒ delete()
        get: ƒ ()
        getAll: ƒ getAll()
        onchange: (...)
        set: ƒ ()
```

发现常用的 API 有：一个事件 onchange，

**注意⚠️**： cookieStore 所有的方法都为 Promise。增（set）删（delete）改（set）查（get/getAll），控制台直接支持 await ，所以直接在谷歌控制台里面调试。

### 获取 Cookie
增（set）删（delete）改（set）查（get/getAll）都支持字符串索引和对象索引：

```js
> await cookieStore.get({ name: "rec_id" })
< {domain: "yuchengkai.cn", expires: 3486867076626, name: "rec_id", path: "/", sameSite: "lax", secure: false, value: "edfc2bce0c9ad01d73b99f66f5bb1c42"}
> await cookieStore.get("rec_id")
< {domain: "yuchengkai.cn", expires: 3486867076626, name: "rec_id", path: "/", sameSite: "lax", …}
```
返回的对象说明：

```js
{ 
    domain: "yuchengkai.cn",    // 域名
    expires: 3486867076626,     // cookie 的过期时间
    name: "rec_id",             // name 字段
    path: "/",                  // 浏览器路径匹配才会加载 cookie
    sameSite: "lax",            // 防止 CSRF
    secure: false,              // 是否开启 https
    value: "edfc2bce0c9ad01d73b99f66f5bb1c42" // value 字段
}
```


指定条件 `{domain: "yuchengkai.cn"}` 获取全部的 cookie：
```js
> await cookieStore.getAll({domain: "yuchengkai.cn"})
< (6) [{…}, {…}, {…}, {…}, {…}, {…}]
    0: {domain: "yuchengkai.cn", expires: 3486867076626, name: "rec_id", path: "/", sameSite: "lax", …}
    1: {domain: "yuchengkai.cn", expires: 1636015817000, name: "Hm_lvt_52ee35894430604d0d3ce3b9c089adf9", path: "/", sameSite: "lax", …}
    2: {domain: "yuchengkai.cn", expires: 1667551817000, name: "_ga", path: "/", sameSite: "lax", …}
    3: {domain: "yuchengkai.cn", expires: 1604566217000, name: "_gid", path: "/", sameSite: "lax", …}
    4: {domain: "yuchengkai.cn", expires: null, name: "Hm_lpvt_52ee35894430604d0d3ce3b9c089adf9", path: "/", sameSite: "lax", …}
    5: {domain: "yuchengkai.cn", expires: 1604479877000, name: "_gat", path: "/", sameSite: "lax", …}
    length: 6
    __proto__: Array(0)
```

`getAll()` 方法不指定参数表示获取所有的 cookie：
```js
> await cookieStore.getAll();
< (5) [{…}, {…}, {…}, {…}, {…}]
0: {domain: "yuchengkai.cn", expires: 1636022429000, name: "Hm_lvt_52ee35894430604d0d3ce3b9c089adf9", path: "/", sameSite: "lax", …}
1: {domain: "yuchengkai.cn", expires: null, name: "Hm_lpvt_52ee35894430604d0d3ce3b9c089adf9", path: "/", sameSite: "lax", …}
2: {domain: "yuchengkai.cn", expires: 1667558429000, name: "_ga", path: "/", sameSite: "lax", …}
3: {domain: "yuchengkai.cn", expires: 1604572829000, name: "_gid", path: "/", sameSite: "lax", …}
4: {domain: "yuchengkai.cn", expires: 1604486489000, name: "_gat", path: "/", sameSite: "lax", …}
length: 5
__proto__: Array(0)
```

### 新增/更改 Cookie

使用 set() 方法，如果浏览器不存在这个 cookie，将会设置一个新的 cookie，如果存在直接更改：

```js
> await cookieStore.set({name: "sex", value: "girl" });
< undefined
> await cookieStore.get("sex");
> {domain: null, expires: null, name: "sex", path: "/", sameSite: "strict", …}
    domain: null
    expires: null
    name: "sex"
    path: "/"
    sameSite: "strict"
    secure: true
    value: "girl"
    __proto__: Object
```

注意不填的可选属性，会有默认值：

```js
The options default to:
    Path: /
    Domain: null
    expires: null
    SameSite: strict
```

### 删除 Cookie

```js
> await cookieStore.set({ "name": "sex", "value": "girl" });
< undefined
> await cookieStore.get("sex");
< {domain: null, expires: null, name: "sex", path: "/", sameSite: "strict", …}
> await cookieStore.delete("sex");
< undefined
> await cookieStore.get("sex");
< null
> await cookieStore.set({ "name": "sex", "value": "boy" });
< undefined
> await cookieStore.get("sex");
< {domain: null, expires: null, name: "sex", path: "/", sameSite: "strict", …}
> await cookieStore.delete({ name: "sex" });
< undefined
> await cookieStore.get("sex");
< null
```

没删掉指定的 cookie，因为删除的时候 cookie 可选属性也是有默认值的，一定要**精确匹配**：

```js
> await cookieStore.set({ "name": "sex", "value": "boy", domain: "yuchengkai.cn" });
< undefined
> await cookieStore.get("sex");
< {domain: "yuchengkai.cn", expires: null, name: "sex", path: "/", sameSite: "strict", …}
> await cookieStore.delete({ name: "sex" });
< undefined
> await cookieStore.get("sex");
< {domain: "yuchengkai.cn", expires: null, name: "sex", path: "/", sameSite: "strict", …}
> await cookieStore.delete({ name: "sex", domain: "yuchengkai.cn" });
< undefined
await cookieStore.get("sex");
null
```

### 监听 Cookie 的改变

```js
> cookieStore.addEventListener("change", event => { console.log(event) });
< undefined
> await cookieStore.get("sex");
< null
> await cookieStore.set({ "name": "sex", "value": "boy" });
< undefined // 事件被触发打印的结果
    VM2426:1 
    CookieChangeEvent {isTrusted: true, changed: Array(1), deleted: Array(0), type: "change", target: CookieStore, …}
    bubbles: false
    cancelBubble: false
    cancelable: false
    changed: [{…}]
    composed: false
    currentTarget: CookieStore {onchange: null}
    defaultPrevented: false
    deleted: []
    eventPhase: 0
    isTrusted: true
    path: []
    returnValue: true
    srcElement: CookieStore {onchange: null}
    target: CookieStore {onchange: null}
    timeStamp: 1060285.8750000596
    type: "change"
    __proto__: CookieChangeEvent
```

当我们设置或更改(set)或删除（delete） Cookie 时，事件就会抛出我们所改变的内容，其中如果是 set 触发的事件， onchange 里面的 event.changed 将会接收，如果是 或删除（delete） 触发的事件，onchange 里面的 event.deleted 将会接收。

### 参考

cookieStore 官网：https://wicg.github.io/cookie-store/#CookieStore
参考链接🔗：https://juejin.im/post/6889231003697709070#comment