---
title: js 错误捕获总结
date: 2019-04-15 15:01:10
tags:
---

大纲
 * try...catch
 * window.onerror
 * window.addEventLister('error',function(){}[,Boolean-IsCapture])

 1. try..catch

 try...catch是在运行时捕获错误，并用catch处理错误。

 但是不能处理异步的错误，promise的错误也捕获不到。

 如果是任何情况下都要执行的代码，可以用 finally 子句。

 ```js
 try {
     console.log(1)
 } catch(error) {
     console.log(2)
 } finally {
     console.log(0)
 }
 ```

 2. window.onerror

 与try catch 的区别，使用try catch错误被捕获了之后，JS会继续进行下去，但是使用window.onerror 虽然会监听到错误，但是JS 会在此处停止运行, 但是try catch 就不会。

 使用方法:
 ```js
 window.onerror = function(message, source, lineno, colno, error){ ... return true/false}
 ```
默认返回的是false， 如果返回true，就会阻止浏览器上面的报错。

**注意** `element.onerror = function(event){}`

这里由于某种历史原因，在window 和element 上监听的函数的参数是不一样的。

同时，这里的错误是不会冒泡到window的错误处理方法上的。

3. window.addEventLister('error',function(){}[,Boolean])

3-1. 事件流

> 题外话，由于理不清楚，这个事件流到底是怎么个回事，已经转头温习事件流去了。话说，这个error的事件怎么个流动法

好吧，万事都得躬行啊。惭愧，这么久了才把这个搞清楚。

首先，addEventListener 最后的参数意思为 useCapture ，是否使用捕获模式，默认为false，使用冒泡模式。

![捕获冒泡](https://images2015.cnblogs.com/blog/776370/201608/776370-20160814181725921-250498467.png)

使用捕获模式就会在捕获阶段触发函数，反之则反。说明一点，目标阶段常被视为冒泡阶段。

捕获会先比冒泡执行。 事件委托是建立在事件冒泡的基础上的。

3-2. stopPropagation()  preventDefault()

上面的两个方法都会讲事件流在使用位置停止。

3-3 e.target  e.currentTarget

currentTarget 是事件流到这里(current)触发函数时候的元素， 而 target 才是事件流发出时候真正的元素

> 回到正题，addEventListener('error',function(){...},...isCapture)

资源加载时候的错误，是不会传到window的 onerror 上面去的。

复习了事件流之后，这个地方的监听资源的报错，得在 **捕获** 阶段(isCapture 为 true)监听才能监听的到。（不知道为啥）

<!-- 尝试解释一下为什么只有在捕获的时候才可以捕捉到error：

在错误监听函数中打印错误Error 对象， 可以发现有一个path的对象，值为 `path: (5) [img#ii, body, html, document, Window]`

我的一个猜想，应该是这个错误对象只有 -->

> error 产生的流，是个怎么样的流向啊，还没有找到。。

> 而且没哟找到让浏览器不输出这个错误的方法，是不是就没有呢。

```js
window.addEventListener('error', (msg, url, row, col, error) => {
    console.log('我知道错误了');
    console.log(
        msg, url, row, col, error
    );
    return true; // 貌似没有啥用
}, true);
```