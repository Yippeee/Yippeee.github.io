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


其实看上来，sticky 的效果像是 absolute 和 fixed 的结合，在判断阈值的时候，是通过和视窗的距离判断；但是在过阈值之后又变成绝对定位。


## window.getComputedStyle() 方法

```js
let style = window.getComputedStyle(ele, [pseudoElement])
```

pseudoElement 是伪元素，没有可以不传或者传null。

返回的值是实时的 `CSSStyleDeclaration` 对象，该对象会实时更新属性。

*这个返回的CSSStyleDeclaration对象是只读的，不能修改。*

### CSSStyleDeclaration 对象

除了上面的getComputedStyle 方法会返回 CSSStyleDeclaration 外， style 属性返回的也是一个 CSSStyleDeclaration 对象。

但是，差别在于，style 只能获取内联的样式（也就是直接写在HTML 标签中的），不过这个对象是*可以修改*的

#### CSSStyleDeclaration.getPropertyPriority(property_name)

返回可选的优先级，”Important", 例如： priString= styleObj.getPropertyPriority('color')

#### CSSStyleDeclaration.getPropertyValue(property_name)

属性名可以使用驼峰命名法。

返回属性值。例如: valString= styleObj.getPropertyValue('color')

#### CSSStyleDeclaration.removeProperty(property_name)

返回被删除的属性。

### CSSStyleDeclaration.setProperty(property_name.property_val[, property_priority])

没有返回值。设置属性值。

*后面两个方法都只有在 style 方法中获取的对象可以使用*

### 影响

使用这个方法会导致页面回流。

## 可以继承的css属性

> 这个问题，记不住啊..得想个办法

继承属性就是，当一个元素没有设定这个属性的值的时候，则取父元素同属性的计算值。

而非继承属性就是，属性没有设定值的时候，就设为属性的初始值。

不可继承的：display、margin、border、padding、background、height、min-height、max-height、width、min-width、max-width、overflow、position、left、right、top、bottom、z-index、float、clear、table-layout、vertical-align、page-break-after、page-bread-before和unicode-bidi。

所有元素可继承：visibility 和 cursor。

内联元素可继承：letter-spacing、word-spacing、white-space、line-height、color、font、font-family、font-size、font-style、font-variant、font-weight、text-decoration、text-transform、direction。

### 总结

可以发现，元素可以继承的属性都是内部样式的属性。例如：颜色，字距，字体，字体顺序。
暂时来个总结，一般可以继承的属性，都是元素内容字体的属性，包括颜色，字体，排版，风格。

visibility 和 cursor 可以继承，也是可以理解的，外部的都不可见了，里面的怎么还能看见呢。

想来也是，如果子元素的这些元素都与父元素不一致，看上去很不舒畅。