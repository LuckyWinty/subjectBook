## NODE错误处理

**理想的错误处理：**

* 出现错误时能将任务中断在一个合适的位置
* 能记录错误的摘要、调用栈及其他上下文
* 通过这些记录能够快速地发现和解决问题

**一些容错手段：**

1、可以用uncaughtException来全局捕获未捕获的Error

```
process.on("uncaughtException", function(e) {
    logger.helper.writeErr({errNo:'process.fatal',errMsg:e,errDetail:e.stack});
    process.send && process.send({
        cmd: "suicide"
    });
    server.close(function() {
        process.exit(1);
    });
    setTimeout(function() {
        process.exit(1);
    }, 5000);
});
```

2、domain模块

3、try...catch（无法捕捉异步回调里的异常）

## 浏览器端`window.onerror`

`window.onerror`是我们在做错误监控中用到比较多的方案。`window.onerror`包含了`try...catch`的优势，而`try...catch`无法捕获的语法错误和全局异常处理，`window.onerror`都可以做到。不过，由于是全局监测，就会统计到浏览器插件中的 js 异常。

`window.onerror`还有一个问题就是浏览器跨域，页面和 js 代码在不同域上时，浏览器出于安全性的考虑，会将异常内容隐藏，我们只能获取到一个简单的`Script Error`信息。解决方案也很简单：

* 给应用内所需的`<script>`标签添加 crossorigin 属性；
* 在 js 所在的 cdn 服务器上添加`Access-Control-Allow-Origin: *`HTTP 头；
* 使用`throw new Error(“error message here”)`。

**其他**

1. Promise不需要使用try...catch，直接使用.catch\(\)
2. Generator 可以直接使用co 函数库来使用try...catch
3. async 和 await 可以直接使用try...catch



