### 卡顿

###### **网页的 FPS**

网页内容在不断变化之中，网页的 FPS 是只浏览器在渲染这些变化时的帧率。帧率越高，用户感觉网页越流畅，反之则会感觉卡顿。

### 监控卡顿方法：

每秒中计算一次网页的 FPS 值，获得一列数据，然后分析。通俗地解释就是，通过 requestAnimationFrame API 来定时执行一些 JS 代码，如果浏览器卡顿，无法很好地保证渲染的频率，1s 中 frame 无法达到 60 帧，即可间接地反映浏览器的渲染帧率。

```
var lastTime = performance.now();
var frame = 0;
var lastFameTime = performance.now();
var loop = function(time) {
    var now =  performance.now();
    var fs = (now - lastFameTime);
    lastFameTime = now;
    var fps = Math.round(1000/fs);
    frame++;
    if (now > 1000 + lastTime) {
        var fps = Math.round( ( frame * 1000 ) / ( now - lastTime ) );
        frame = 0;    
        lastTime = now;    
    };           
    window.requestAnimationFrame(loop);   
}
```

更多：[https://zhuanlan.zhihu.com/p/39292837](https://zhuanlan.zhihu.com/p/39292837)

#### 监控网页崩溃的方法：

**1、基于 Service Worker 的崩溃统计方案**

随着 PWA 概念的流行，大家对 Service Worker 也逐渐熟悉起来。基于以下原因，我们可以使用 Service Worker 来实现网页崩溃的监控：

1. Service Worker 有自己独立的工作线程，与网页区分开，网页崩溃了，Service Worker 一般情况下不会崩溃；
2. Service Worker 生命周期一般要比网页还要长，可以用来监控网页的状态；
3. 网页可以通过
   **navigator.serviceWorker.controller.postMessage**
   API 向掌管自己的 SW 发送消息。

基于以上几点，我们可以实现一种基于**心跳检测**的监控方案：

* p1：网页加载后，通过**postMessage**API 每**5s**给 sw 发送一个心跳，表示自己的在线，sw 将在线的网页登记下来，更新登记时间；
* p2：网页在**beforeunload**时，通过**postMessage**API 告知自己已经正常关闭，sw 将登记的网页清除；
* p3：如果网页在运行的过程中 crash 了，sw 中的**running**状态将不会被清除，更新时间停留在奔溃前的最后一次心跳；
* sw：Service Worker 每**10s**查看一遍登记中的网页，发现登记时间已经超出了一定时间（比如 15s）即可判定该网页 crash 了。

更多：https://zhuanlan.zhihu.com/p/40273861



