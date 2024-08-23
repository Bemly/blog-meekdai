## 起初

最开始的js是用于浏览器的快速脚本语言

动态、宽松语法让许多代码得于以BUG的方式运行起来（（（

但是最开始作为一个脚本语言，并没有考虑到js发展规模之快，以至于ES标准起初没有模块化的要求

随着外部js引入的逐渐增多，各类js的变量出现了冲突

为了解决这个办法，一部分是通过闭包，将自己的变量封装在一个大家基本不会使用的变量名上

比如 jQuery 这名老将，就以自己独特的占有率占用了$这个大变量

模块一多就不太好管理了，譬如现在的Node.js的npm一拉取webpack差不多就有150多个包

这么多模块的变量冲突在处理依赖的时间恐怕要比实际引入开发多得多

此时除了手动闭包冒出变量之外有了新的选择：那就是采用AMD加载器

例如像使用 RequireJS 这种 AMD 加载器，利用define()语法来

自动化帮我们闭包封装各自所需要的模块。

## 变革

随着谷歌花大价钱投入的V8引擎受到各种吹捧之后，

基于 Nodejs 的 JS 运行环境应运而生，

他类似 JVM、Rust标准库 一样，把电脑上的各类接口封装为统一的 API

例如：fs、http、fetch(> Node.js v13) 等打包好的轮子

通过 V8 引擎的跨平台通用性，我们可以很方便的翻阅文档，轻松调用这些 API

一套代码，只要是个能够支持浏览器运行的环境（主要是支持V8引擎）都可以顺利运行

在 Node.js 上，node开发团队不满足于这个抽象的AMD加载器

采用了CJS（CommonJS）模块化语法，不再使用define而是module.export这类关键字来构建模块

虽然js语法都一致，但是这造成了浏览器和Node.js的愈来愈割裂

## 统一

为了让Nodejs和浏览器能够同时支持CJS和AMD标准

更抽象的UMD模块化统一兼容层诞生了

本质上还是通过 Nodejs、AMD模块加载器、原生浏览器 对typeof的解析值不同所采用不同的方法

太暴力了，之后就被ESM拉清单了
```js
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD. Register as an anonymous module.
        define(['b'], factory);
    } else if (typeof module === 'object' && module.exports) {
        // Node. Does not work with strict CommonJS, but
        // only CommonJS-like environments that support module.exports,
        // like Node.
        module.exports = factory(require('b'));
    } else {
        // Browser globals (root is window)
        root.returnExports = factory(root.b);
    }
}(typeof self !== 'undefined' ? self : this, function (b) {
    // Use b in some fashion.

    // Just return a value to define the module export.
    // This example returns an object, but the module
    // can return a function as the exported value.
    return {};
}));

```

## 终结
ES2015之后，以import export为模块化语法糖的ESM标准逐渐扩散，node、浏览器、typescript逐渐采用官方模块化ESM语法，但是对于老的模块，采用兼容模式，ESM和CJS混合在一些框架适配上仍无法正常工作

我们需要更多的UMD 2.0（（（

因为我最喜欢的coffeescript，因为node的vm模块还在古老的CJS语法，不能正常运行，气死偶嘞