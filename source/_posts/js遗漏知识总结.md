---
title: js遗漏知识总结
date: 2019-04-09 14:14:02
tags:
---

## 不得不加上的分号

现在在写js的时候，基本上已经会省略掉分号（;）的使用，但是在有些情况省略掉分号就会导致错误。

先直接下结论，在使用 `+` `-` `(` `[` `/` 位于行首的时候需要加上分号。

除了这五种情况外，js会在行尾自动加上分号，但是上面五种情况不会，有些时候就会导致错误。

下列两种情况使用中常出现：

```js
;[arr[1], arr[2]] = [arr[2], arr[1]]
```
```js
;(function(){
    // ....
})()
```
这种时候都需要手动加上分号

## defer async 在script 标签中的作用以及差异

首先，defer 和 async 都是异步加载脚本文件的语法。都只能作用于有src 属性的script 标签

区别在于defer 会在文档解析完成之后，也就是DOMContentLoaded 事件前执行，

而async 在脚本下载完成之后就执行，中断文档的渲染

多个defer 会按照顺序加载执行，而 async会依据下载时间

## 解构赋值使用方法总结

解构赋值，让获取变量的值，更加的优雅，代码更加的易读。

> 小提一句原理，是利用的新增的iterator 接口。

### 数据的解构赋值

常规的情况：
```js
let [a,b,c] = [1,2,3]
```
如果需要忽略某些值，用,,来忽略
```js
let [a,,,d] = [1,2,3,4] // a=a,d=4
```
设定默认值:
```js
let [a=5,b=5] = [2] // a=2,b=5
```
剩余参数的赋值；（使用... , 但是注意，... 一定得是最后一个参数; 如果后面还有参数，请使用,,, 的方法）
```js
let [a,b,...rest] = [1,2,3,4,5] // a = 1, b = 2, rest = [3,4,5]
```
#### 经常使用的场景

下面记录几种经常在使用中利用的方法：

1. 交换变量
```js
[a,b] = [b,a]
```

2. 正则表达式的exec语句、函数参数argument中某个值的获取

exec会返回一个带有很多值的数组，数组中有些的值，是我们不需要的。就可以直接使用解构获取想要的值

### 对象的解构赋值

普通的情况:
```js
let {o, p} = {o:'oVAl',l:'lVAL'} // o = 'oVAl', p = undefined
```
使用对象的解构赋值，左边的值必须是对象中的属性名,如果不是将获取不到值(undefined)。如果需要重命名得使用下面的方法：
```js
let {o:oo, p} = {o:'oVaL',p:'pVAL'} // oo = 'oVAl', p = 'pVAL'
```
注意，如果是没有声明的赋值，需要用()包起来，并且注意使用;(原因上面有讲)
```js
;({o, p} = {o:'oVaL',p:'pVAL'});
```
...rest 和 默认值的使用方法就和数组的差不多了 

## requestAnimationFrame 理解

这是web中专门对优化动画，增加的函数。按照翻译，API的名字应该叫做，**请求动画帧**。

接受一个callback 回调，在屏幕刷新时调用。一般在callback 中还会继续调用requestAnimationFrame, 直到约定的条件达成后，实现动画的效果。

使用注意事项。requestAnimationFrame是window调用的，这一点和setTimeout一眼的。如果要使用this的话，通常可以在外面包装一个函数，用bind绑定this。

例如下面通过sayBind，来保留this
```js
class test{
    constructor(){
        this.name = name
        this.sayBind = this.say.bind(this)
        requestAnimationFrame(this.sayBind)
    }
    say(){
        requestAnimationFrame(this.sayBind)
    }
}
```

### 屏幕刷新频率

一般的显示器设置的是60HZ，就是一秒钟刷新60次，16.67ms刷新一次。

### 与setTimeout的对比

在没有requestAnimationFrame之前，我们都是使用setTimeout来实现动画的。那为什么使用setTimeout不好呢？

1. 屏幕刷新的频率只是一般来说是60HZ，但是不是绝对的。这个间隔人为设定，会有误差。

2. setTimeout 是在异步队列中执行的，在主线程任务执行完成之后才回去执行。

以上两点，会导致setTimeout的执行步调和屏幕刷新不一致，会导致动画的丢帧。

同时requestAnimationFrame对性能也更加的友好，页面处于未激活状态下，requestAnimationFrame 不会调用，不同于setTimeout。

综上，如果有使用动画的地方，使用requestAnimationFrame 而不是setTImeout

## 鼠标事件中的那些 X 和 Y 

由于些历史原因，事件中定义了很多的位置信息的属性。在使用和阅读中，经常容易混淆，在这里把它们都拿出来做一个总结。

随便在浏览器的控制台中打印一个事件的event，就可以看到，有这些与位置或者尺寸信息相关的属性。

`clientX` `clientY` `layerX` `layerY` `movementX`  `movementY` `offsetX` `offsetY` `pageX` `pageY` `screenX` `screenY` `x` `y`

咋眼一看，是有点多啊。。。

### clientX, clientY

浏览器内容区域的X/Y坐标（不包含滚动条）

### screenX,screenY

屏幕中的X/Y坐标

### pageX, pageY

整个页面的X/Y坐标，包括可以滚动区域。（在没有滚动的情形下，这个值与client的数值相同）

### layerX、layerY

针对于绝对定位的元素来使用的位置属性。非绝对定位时，与pageX/pageY数值相同。

绝对定位元素的时候，是基于元素左上角的位置。

### movementX、movementY

这只是 `mousemove` 事件才有的属性。 值就是上一次的值和这一次的差值。

等价于 `currentEvent.movementY = currentEvent.screenY - previousEvent.screenY.`

### offsetX、offsetY

offset 获取的数值都是相对于父容器的值。

同理的还有 offsetWidth offsetHeight offsetTop offsetLeft

### x,y

这只是 clientX 和 clientY 的别名而已。

## canvas中移动幻影效果

> 幻影效果啊，这是我小时候玩泡泡堂最想要的皮肤。小学的时候，真的被这个帅到不行...

在一般的canvas动画中，清除上一帧，使用的方法一般是 `clearRect()`

如果想出现幻影的效果的话，应该保留上几帧的画面，我们使用半透明的遮布去覆盖，就只会让上几帧的画面颜色变淡，到达幻影的效果。

`ctx.fillStyle = 'rgba(255,255,255,0.6)';ctx.fillRect(0,0,width,height)`