# 如何取消 Http 请求

**前言：**如何取消 Http 请求？这是一个经典的面试题了，平时工作中大家应该很少用到。但是某些时候还是挺有用的，比如：

- 比如点击一个按钮，这个按钮暂且就叫运算按钮吧，它需要后端完成运算之后返回数据，在等待期间，用户有可能又点击了。
- 常见的筛选器，查询条件过多的话，用户也要等待，如果在等待期间，用户又输入了一个特简单的查询条件，就会导致查询条件和数据渲染不一致的问题。

使用 Loading 图罩住浏览器窗口，禁止用户操作。

### XHR

xhr.abort();
https://www.cnblogs.com/xiaoyuxy/p/12346344.html


### Fetch

https://github.com/github/fetch#aborting-requests


### Axios

https://www.npmjs.com/package/axios#cancellation

### isomorphic-fetch

https://github.com/github/fetch#aborting-requests


### 手把手封装axios取消重复请求


https://shuliqi.github.io/shuliqi.github.io/2020/08/31/%E5%B0%81%E8%A3%85axios%E5%8F%96%E6%B6%88%E9%87%8D%E5%A4%8D%E8%AF%B7%E6%B1%82/#axios-%E5%A6%82%E4%BD%95%E5%8F%96%E6%B6%88%E8%AF%B7%E6%B1%82