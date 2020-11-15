# 如何让一个盒子水平垂直居中

分三部分：

1. 水平垂直居中
2. 水平居中
3. 垂直居中
4. 两端对齐

### 水平垂直居中

1. absolute + margin: auto;

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>水平垂直居中</title>
    <style>
        div {
            width: 200px;
            height: 200px;
            background-color: aqua;
            position: absolute;
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            margin: auto;
        }
    </style>
</head>
<body>
    <div></div>
</body>
</html>
```

2. absolute +  (-margin)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>水平垂直居中</title>
    <style>
        div {
            width: 200px;
            height: 200px;
            background-color: aqua;
            position: absolute;
            left: 50%;
            top: 50%;
            margin-left: -100px;
            margin-top: -100px;
        }
    </style>
</head>
<body>
    <div></div>
</body>
</html>
```

3. absoluete + transform

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>水平垂直居中</title>
    <style>
        div {
            width: 200px;
            height: 200px;
            background-color: aqua;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
        }
    </style>
</head>
<body>
    <div></div>
</body>
</html>
```
4. vh/vw + calc

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>水平垂直居中</title>
    <style>
        div {
            width: 200px;
            height: 200px;
            background-color: aqua;
            margin-top: calc((100vh - 200px) / 2);
            margin-left: calc((100vw - 200px) / 2);
        }
    </style>
</head>
<body>
    <div></div>
</body>
</html>
```

5. flex 布局

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>水平垂直居中</title>
    <style>
        section {
            width: 600px;
            height: 600px;
            background-color: aqua;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        main {
            width: 200px;
            height: 200px;
            background-color: yellow;
        }
    </style>
</head>
<body>
    <section>
        <main></main>
    </section>
</body>
</html>
```

6. grid 布局

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>水平垂直居中</title>
    <style>
        section {
            width: 600px;
            height: 600px;
            background-color: aqua;
            display: grid;
            grid-template-columns: 200px;
            grid-template-rows: 200px;
            justify-content: center;
            align-content: center;
        }
        main {
            background-color: yellow;
        }
    </style>
</head>
<body>
    <section>
        <main></main>
    </section>
</body>
</html>
```

7. flex/grid 与 margin:auto (最简单写法)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>水平垂直居中</title>
    <style>
        section {
            width: 600px;
            height: 600px;
            background-color: aqua;
            display: flex;
        }
        main {
            width: 200px;
            height: 200px;
            margin: auto;
            background-color: yellow;
        }
    </style>
</head>
<body>
    <section>
        <main></main>
    </section>
</body>
</html>
```
8. text-align + line-height + vertical-align

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>水平垂直居中</title>
    <style>
        section {
            width: 600px;
            line-height: 600px;
            background-color: aqua;
            text-align: center;
        }
        main {
            display: inline-block;
            vertical-align: middle;
            width: 200px;
            height: 200px;
            background-color: yellow;
        }
    </style>
</head>
<body>
    <section>
        <main></main>
    </section>
</body>
</html>
```

参考：[如何居中一个元素（终结版）](https://github.com/ljianshu/Blog/issues/29)