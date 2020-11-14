# RFC 

> 记录一些 VUE3 的新特性。

### What is an RFC?

[RFC（request for comments）](https://github.com/vuejs/rfcs)，简单来讲就是一些新特性在正式加入框架之前，放出来让大家讨论一下，征求征求开发者的意见。


### 标签语法

标签语法主要是和 while 和 for 循环结合使用，用在普通语法中，就相当于声明了一个全局变量。

[lable statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label)

```js
// 可重复声明
state: name = "Condor Hero";
state: age = "18";
console.log(name, age);
```

vue 中的 setup：
```vue
<script setup>
    // imported components are also directly usable in template
    import Foo from './Foo.vue'
    import { ref } from 'vue'

    // write Composition API code just like in a normal setup()
    // but no need to manually return everything
    const count = ref(0)
    const inc = () => { count.value++ }
</script>

<template>
    <Foo :count="count" @click="inc" />
</template>
```
ref sugar：

```vue
<script setup>
    // declaring a variable that compiles to a ref
    ref: count = 1

    function inc() {
    // the variable can be used like a plain value
    count++
    }

    // access the raw ref object by prefixing with $
    console.log($count.value)
</script>

<template>
    <button @click="inc">{{ count }}</button>
</template>

```

### CSS variable

可以在 CSS 中使用 JS 变量：[CSS variables}(https://github.com/vuejs/rfcs/pull/226)

```vue
<template>
    <div class="text">hello</div>
</template>

<script>
export default {
    data() {
        return {
            color: 'red'
        }
    }
}
</script>

<style vars="{ color }">
.text {
    color: var(--color);
}
</style>
```


