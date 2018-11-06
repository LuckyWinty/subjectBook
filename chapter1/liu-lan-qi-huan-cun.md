### 缓存分类

##### 缓存位置

* Service Worker

* Memory Cache

* Disk Cache

* 网络请求

**按照获取资源时请求的优先级依次排列**

1. Memory Cache

2. Service Worker Cache

3. HTTP Cache

4. Push Cache

## Meta标签

（1）使用HTML Meta 标签

```
<meta http-equiv="Pragma" content="no-cache">
```

上述代码的作用是告诉浏览器当前页面不被缓存，每次访问都需要去服务器拉取。但是:

  a. 仅有IE才能识别这段meta标签含义，其它主流浏览器仅识别“Cache-Control: no-store”的meta标签。

  b. 在IE中识别到该meta标签含义，并不一定会在请求字段加上Pragma，但的确会让当前页面每次都发新请求_（仅限页面，页面上的资源则不受影响）_。

## HTTP 缓存机制

HTTP 缓存是我们日常开发中最为熟悉的一种缓存机制。它又分为**强缓存**和**协商缓存**。优先级较高的是强缓存，在命中强缓存失败的情况下，才会走协商缓存。

图解过程：

![](/assets/httpprocess.png)

### 强缓存的特征

强缓存是利用 http 头中的 Expires 和 Cache-Control 两个字段来控制的。强缓存中，当请求再次发出时，浏览器会根据其中的 expires 和 cache-control 判断目标资源是否“命中”强缓存，若命中则直接从缓存中获取资源，不会再与服务端发生通信。

### 强缓存的实现：从 expires 到 cache-control

1、实现强缓存，过去我们一直用`expires`。当服务器返回响应时，在 Response Headers 中将过期时间写入 expires 字段。expires 是一个时间戳，接下来如果我们试图再次向服务器请求资源，浏览器就会先对比本地时间和 expires 的时间戳，如果本地时间小于 expires 设定的过期时间，那么就直接去缓存中取这个资源。它最大的问题在于对“本地时间”的依赖。

2、Cache-Control 可以视作是 expires 的**完全替代方案**。在当下的前端实践里，我们继续使用 expires 的唯一目的就是

**向下兼容**。在 Cache-Control 中，我们通过`max-age`来控制资源的有效期。**Cache-Control 相对于 expires 更加准确，它的优先级也更高。当 Cache-Control 与 expires 同时出现时，我们以 Cache-Control 为准。s-maxage 优先级高于 max-age，两者同时出现时，优先考虑 s-maxage。如果 s-maxage 未过期，则向代理服务器请求其缓存内容。s-maxage仅在代理服务器中生效，客户端中我们只考虑max-age。**

3、Cache-Control中的值no-cache 绕开了浏览器：我们为资源设置了 no-cache 后，每一次发起请求都不会再去询问浏览器的缓存情况，而是直接向服务端去确认该资源是否过期。Cache-Control中的值no-store 比较绝情，顾名思义就是不使用任何缓存策略。在 no-cache 的基础上，它连服务端的缓存确认也绕开了，只允许你直接向服务端发送请求、并下载完整的响应。

### 协商缓存：浏览器与服务器合作之下的缓存策略

协商缓存机制下，浏览器需要向服务器去询问缓存的相关信息，进而判断是重新发起请求、下载完整的响应，还是从本地获取缓存的资源。 如果服务端提示缓存资源未改动（Not Modified），资源会被重定向到浏览器缓存，这种情况下网络请求对应的状态码是 304。

### 协商缓存的实现：从 Last-Modified 到 Etag

1、Last-Modified 是一个时间戳，如果我们启用了协商缓存，它会在首次请求时随着 Response Headers 返回。随后我们每次请求时，会带上一个叫 If-Modified-Since 的时间戳字段，它的值正是上一次 response 返回给它的 last-modified 值。服务器接收到这个时间戳后，会比对该时间戳和资源在服务器上的最后修改时间是否一致，从而判断资源是否发生了变化。如果发生了变化，就会返回一个完整的响应内容，并在 Response Headers 中添加新的 Last-Modified 值；否则，返回如上图的 304 响应，Response Headers 不会再添加 Last-Modified 字段。但是有些场景下服务器并没有正确感知文件的变化。例如没改动或改动太快。

2、Etag 是由服务器为每个资源生成的唯一的**标识字符串**，这个标识字符串是基于文件内容编码的，只要文件内容不同，它们对应的 Etag 就是不同的，反之亦然。因此 Etag 能够精准地感知文件的变化。Etag 的生成过程需要服务器额外付出开销，会影响服务端的性能，这是它的弊端。因此启用 Etag 需要我们审时度势。正如我们刚刚所提到的——Etag 并不能替代 Last-Modified，它只能作为 Last-Modified 的补充和强化存在。 Etag 在感知文件变化上比 Last-Modified 更加准确，优先级也更高。当 Etag 和 Last-Modified 同时存在时，以 Etag 为准。

## MemoryCache

MemoryCache，是指存在内存中的缓存。从优先级上来说，它是浏览器最先尝试去命中的一种缓存。从效率上来说，它是响应速度最快的一种缓存。 内存缓存是快的，也是“短命”的。它和渲染进程“生死相依”，当进程结束后，也就是 tab 关闭以后，内存里的数据也将不复存在。资源存不存内存，浏览器秉承的是“节约原则”。例如Base64 格式的图片、体积不大的 JS、CSS 文件等。

## Service Worker Cache

Service Worker 是一种独立于主线程之外的 Javascript 线程。它脱离于浏览器窗体，因此无法直接访问 DOM。这样独立的个性使得 Service Worker 的“个人行为”无法干扰页面的性能，这个“幕后工作者”可以帮我们实现离线缓存、消息推送和网络代理等功能。我们借助 Service worker 实现的离线缓存就称为 Service Worker Cache。Service Worker 的生命周期包括 install、active、working 三个阶段。一旦 Service Worker 被 install，它将始终存在，只会在 active 与 working 之间切换，除非我们主动终止它。

**PS：**注意 Server Worker 对协议是有要求的，必须以 https 协议为前提。

## Push Cache

* Push Cache 是指 HTTP2 在 server push 阶段存在的缓存。
* Push Cache 是缓存的最后一道防线。
* 浏览器只有在 Memory Cache、HTTP Cache 和 Service Worker Cache 均未命中的情况下才会去询问 Push Cache。
* Push Cache 是一种存在于会话阶段的缓存，当 session 终止时，缓存也随之释放。 不
* 同的页面只要共享了同一个 HTTP2 连接，那么它们就可以共享同一个 Push Cache。



[https://www.cnblogs.com/slly/p/6732749.html](https://www.cnblogs.com/slly/p/6732749.html)

[https://www.cnblogs.com/shixiaomiao1122/p/7591556.html](https://www.cnblogs.com/shixiaomiao1122/p/7591556.html)

[https://www.cnblogs.com/etoah/p/5579622.html](https://www.cnblogs.com/etoah/p/5579622.html)

