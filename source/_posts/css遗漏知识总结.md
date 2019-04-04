---
title: css遗漏知识总结
date: 2019-04-03 10:04:22
tags:
---

## class匹配符
  
```css
[class$='xxx']{

}

[class^='xxx']{
    
}
```

这两种对类匹配都可以对名称进行帅选。

## sticky 布局

使用sticky可以轻易的实现固定表头类似的需求。使用方法:

```css
.demo{
    position:sticky;
    top: 200px
}
```

在使用position 为 sticky的时候，还需要指定 `left` `right` `top` `bottom` 四个值中的任意一个。

指定的这个值，会作为一个阈值，当到达这个值之后，就会被固定在这个位置。

### 注意事项

1. sticky 的元素不会脱离其父元素的，就是会一直在父元素内部。如果父元素不在可视范围之内，也是会消失的。

2. sticky 的元素没有脱离文档流，不会导致后面的元素占用其位置。

3. 其父元素必须得可以scroll，才可以生效。

4. 左边(left)的优先级大于右边(right)，上面(top)的大于下面(bottom)的


其实看上来，sticky的效果像是absolute 和 fixed 的结合，在判断阈值的时候，是通过和视窗的距离判断；但是在过阈值之后又变成绝对定位。