---
title: CSS布局
date: 2019-03-12 17:16:24
tags:
---

### 两栏布局
> 两栏布局的要求一般是要求一左一右铺满整个界面,同时,左边(或者是右边)的元素固定宽度,另一边的元素随着浏览器的变化而变化

页面布局:(左侧固定300px)
```
<body>
    <div id="left"></div>
    <div id="right"></div>
</body>
```
#### float方法
```css
body{
    height:100px;
}
#left{
    float:left;
    height:100%;
    width:300px;
}
#right{
    margin-right:300px;
    height:100%;
}
```
#### flex方法
```css
body {
    height: 300px;
    display: flex;
    flex-direction: row;
}

#left {
    flex: 0 0 300px;
    height: 100%;
    background-color: red;
}

#right {
    flex: 1 1 auto;
    height: 100%;
    background-color: blue;
}
```

### 三栏布局

### 垂直水平居中