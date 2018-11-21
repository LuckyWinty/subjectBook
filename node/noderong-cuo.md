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

2、try...catch（无法捕捉异步回调里的异常）





