---
title: CSS布局
date: 2019-03-13 11:16:24
tags: css
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

```html
<div id="parent">
    <div id="main"></div>
</div>
```

#### 水平居中

>水平居中分为两种情况,一种是DIV宽度一定的,一种是宽度不定的

##### 对于宽度已知的水平居中

```css
#main{
    display: inline-block;
    width: 200px;
    height: 200px
}
#parent{
    test-align:center;
}
```
在宽度已知的情况,只需要在父元素上加上`test-align:center;`,甚至没有父元素也可以加一个div

##### 对于宽度未知的水平居中

使用 `margin: 0 auto`;

#### 水平垂直居中

##### 绝对定位

```css
#main{
    position: relative;
}
#parent{
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%)
}
```

绝对定位的思想就是让子元素基于父元素进行绝对定位

##### flex布局

```css
#main{
    height: 300px;
    background-color: grey;
    display: flex;
    justify-content: center;
    align-items: center;
}
#parent{

}
```

使用flex布局在对对齐的设置就是主要在父元素上的设置,看的文章都是以前的,都会提到兼容性的问题,但是感觉现在都兼容的很好了.那些之前的hack的方法,实在是不想用了