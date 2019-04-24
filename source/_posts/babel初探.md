---
title: babel初探
date: 2019-04-18 11:07:01
tags:
---

# babel

## babel 做了什么

babel 就是将js中新的语法转化成低版本的JS，一般转成ES5大部分浏览器就够了。

## 运行方式

分为三个阶段：解析，转换，生成。 其中的具体功能是利用AST实现的。

### 插件

babel本身不具有转化的功能，转化的功能都被分解到plugin中去了。这样利用控制，同时在使用babel时候，配置就是必不可少的了。

同时，插件还分为两种：

1. 语法插件

语法插件主要是用于新语法中一些新提供的语法，在老版本中，会认为是错误的语法，而在新版中认为是可以使用的语法。使用语法插件后，就不会报错，会认为这样是正确的。

和转译插件的差别在于是**否转化**，语法插件只是改变了**解析**的规则

2. 转译插件

转译插件插件则是为了支持新式的代码，将它转化成浏览器支持的版本。例如箭头函数：

In: 
```js
var a = () => {};
var b = () => b;
```
经过转译插件之后，Out:
```js
var a = function() {};
var b = function () {return b;};
```

**注意的是，转译插件中会用到语法插件的情况，转译插件会直接启动，不同我们同时制定了**

### 使用插件的方式

就是NPM包引入标准的步骤：

1. npm i babel-plugin-xxx 安装
2. 把插件名字增加到配置文件中

#### 配置文件

babel的配置文件一般有两种方式，根据使用场景选择。

1. babel.config.js

放置于项目的根目录，一般写法：
```js
modele.exports = function(api) {
    api.cache(true);

    const presets = [ ... ];
    const plugins = [ ... ];

    return {
        presets,
        plugins
    };
}
```

2. .babelrc

```json
{
  "presets": [...],
  "plugins": [...]
}
```

这个时候肯定会觉得，那一个项目得用到的插件肯定很多啊，这么一个一个的配置肯定很麻烦啊。

#### preset

so，preset(预设)就出来啦。 其实preset就是一整套插件的方案。

这样我们就不再需要一个一个的配置，而是只设置preset 就可以了。 可以设置的值有：

1. 官方内容

@babel/preset-env
@babel/preset-flow
@babel/preset-react
@babel/preset-typescript

这都是一些常用环境的preset

2. stag-x 

stage-x 每年都会随着js新版本的提案发生变化。分为5个阶段

Stage 0 - 设想（Strawman）：只是一个想法，可能有 Babel插件。
Stage 1 - 建议（Proposal）：这是值得跟进的。
Stage 2 - 草案（Draft）：初始规范。
Stage 3 - 候选（Candidate）：完成规范并在浏览器上初步实现。
Stage 4 - 完成（Finished）：将添加到下一个年度版本发布中。

#### 执行顺序

preset 中的执行顺序是从后往前进行的。和插件的执行顺序是相反的。


## @babel/polyfill

babel 的作用已经说过，是转化js代码，但是它只是针对其语法进行转化。如果是新的API，例如新增的generator、promise 之类的全局对象，babel是不会转化的。

需要使用新的全局对象的时候，需要使用到 `@babel/polyfill`.

但是使用 `@babel/polyfill` 有几点缺点：

1. 导致打包尺寸很大，本身这个插件尺寸就很大。但是很多时候，我们不会全部使用其中的功能。

2. 会污染很多的全局变量。这点只在于本身也在开发类库的时候有影响。

所以，我们通常不会直接使用 `@babel/polyfill`。

### @babel/runtime & @babel/plugin-transform-runtime

