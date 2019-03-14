---
title: HTML&CSS知识补充
date: 2019-03-13 15:35:34
tags:
---
CSS 渐变属性使用
使用渐变属性(gradient)可以一个元素多种颜色
```css
div{
    background-image:linear-gradient(to bottom,red 0%,red 50%,blue 50%,blue 100%)
}
```
因为本身属性是渐变的,所有需要对同一个位置(除了首位之外)需要把结束和开始的颜色都标注出来,就可以达到锐化的效果.
