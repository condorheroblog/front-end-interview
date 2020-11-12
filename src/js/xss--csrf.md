# 浅说 XSS 和 CSRF

### 分别用一句话说清 XSS 和 CSRF？

分别用一句话说清 XSS 和 CSRF？

- XSS 利用浏览器可以无差别执行脚本。
- CSRF 利用浏览器发送一个正常的 Ajax 请求默认携带 cookie。

归根结底都是同源策略的问题。

### 参考
1. [为什么cookie会有 httponly属性?-真实案例解释XSS的三种攻击](https://juejin.im/post/6857698580817182728#heading-3) 注意⚠️开启 HttpOnly 可以 禁用 JavaScript 的 document.cookie API。
2. [为什么cookie会有sameSite属性?-真实案例解释CSRF的三种攻击方式](https://juejin.im/post/6859276462504017927#heading-4)

看完上面两篇文章基本就懂了。下面作为补充。

3. [Cookie 的 SameSite 属性](http://www.ruanyifeng.com/blog/2019/09/cookie-samesite.html)
4. [浅说 XSS 和 CSRF](https://github.com/dwqs/blog/issues/68#issue-341323658)

