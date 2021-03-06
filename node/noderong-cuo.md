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

### 错误分类

* 可预见的还是不可避免的，是操作错误还是bug。
* 操作错误：函数参数类型错误、缺少参数、参数无效等

* 数据错误、接口错误、接口超时等

## 浏览器端`window.onerror`

可以做的异常处理：

1. `JS`语法错误、代码异常
2. `AJAX`请求异常
3. 静态资源加载异常
4. `Promise`异常
5. `Iframe`异常
6. 跨域 Script error
7. 崩溃和卡顿

对应方案：

1.可疑区域增加 Try-Catch

2.全局监控 JS 异常 window.onerror

3.全局监控静态资源异常 window.addEventListener

4.捕获没有 Catch 的 Promise 异常：unhandledrejection

5.VUE 全局捕获错误方法errorHandler和组件级错误捕获方法errorCaptured 和 React componentDidCatch

6.监控网页崩溃：window 对象的 load 和 beforeunload

7.跨域 crossOrigin 解决

`window.onerror`是我们在做错误监控中用到比较多的方案。`window.onerror`包含了`try...catch`的优势，而`try...catch`无法捕获的语法错误和全局异常处理，`window.onerror`都可以做到。不过，由于是全局监测，就会统计到浏览器插件中的 js 异常。

`window.onerror`还有一个问题就是浏览器跨域，页面和 js 代码在不同域上时，浏览器出于安全性的考虑，会将异常内容隐藏，我们只能获取到一个简单的`Script Error`信息。解决方案也很简单：

* 给应用内所需的`<script>`标签添加 crossorigin 属性；
* 在 js 所在的 cdn 服务器上添加`Access-Control-Allow-Origin: *`HTTP 头；
* 使用`throw new Error(“error message here”)`。

**其他**

1. Promise不需要使用try...catch，直接使用.catch\(\)
2. Generator 可以直接使用co 函数库来使用try...catch
3. async 和 await 可以直接使用try...catch

更多：

[http://jartto.wang/2018/11/20/js-exception-handling/](http://jartto.wang/2018/11/20/js-exception-handling/)

