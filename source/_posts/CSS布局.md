---
title: CSS布局
date: 2019-03-12 17:16:24
tags:
---

### 两栏布局
> 两栏布局的要求一般是要求一左一右铺满整个界面,同时,左边(或者是右边)的元素固定宽度,另一边的元素随着浏览器的变化而变化

页面布局:(左侧固定300px)
```html
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
>三栏布局同时也是经常出现的考点之一.一般的情形是,两边的模块是固定的,中间的模块随着浏览器的宽度适应.

### margin 和 利用BFC布局
>这两种布局都有一个确定,就是主体元素需要最后加载.
```html
<div id="parent">
    <div id="left"></div>
    <div id="right"></div>
    <div id="main"></div>
</div>
```

```css
    #parent{
      height: 200px;
    }
    #left{
      display: float;
      float: left;
      width: 300px;
      height: 100%;
      background-color: red;
    }
    #right{
      display: float;
      float: right;
      width: 300px;
      height: 100%;
      background-color: green;
    }
    #main{
      height: 100%;
      background-color: blue;
      /* 这里有两种做法,一种是用margin,一种是用BFC */
      /* margin: 0 300px; */
      overflow: hidden;
    }
```

#### flex布局
>使用flex布局就看起来易用,舒适多了
```html
  <div id="parent">
    <div id="left"></div>
    <div id="main">
      <p style="text-align:center">我们是中间的啊</p>
    </div>
    <div id="right"></div>
  </div>
```
```css
    #parent {
      height: 200px;
      display: flex;
    }
    #left {
      flex: 0 0 300px;
      background-color: red;
    }
    #right {
      flex: 0 0 300px;
      background-color: green;
    }
    #main {
      flex: 1 1 auto;
      background-color: blue;
    }
```
### 垂直水平居中