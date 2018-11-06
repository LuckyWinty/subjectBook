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

## HTTP 缓存机制

HTTP 缓存是我们日常开发中最为熟悉的一种缓存机制。它又分为**强缓存**和**协商缓存**。优先级较高的是强缓存，在命中强缓存失败的情况下，才会走协商缓存。

图解过程：

![](/assets/httpprocess.png)

### 强缓存的实现：从 expires 到 cache-control

1、实现强缓存，过去我们一直用`expires`。当服务器返回响应时，在 Response Headers 中将过期时间写入 expires 字段。expires 是一个时间戳，接下来如果我们试图再次向服务器请求资源，浏览器就会先对比本地时间和 expires 的时间戳，如果本地时间小于 expires 设定的过期时间，那么就直接去缓存中取这个资源。它最大的问题在于对“本地时间”的依赖。

2、Cache-Control 可以视作是 expires 的**完全替代方案**。在当下的前端实践里，我们继续使用 expires 的唯一目的就是

**向下兼容**。在 Cache-Control 中，我们通过`max-age`来控制资源的有效期。**Cache-Control 相对于 expires 更加准确，它的优先级也更高。当 Cache-Control 与 expires 同时出现时，我们以 Cache-Control 为准。s-maxage 优先级高于 max-age，两者同时出现时，优先考虑 s-maxage。如果 s-maxage 未过期，则向代理服务器请求其缓存内容。s-maxage仅在代理服务器中生效，客户端中我们只考虑max-age。**

3、Cache-Control中的值no-cache 绕开了浏览器：我们为资源设置了 no-cache 后，每一次发起请求都不会再去询问浏览器的缓存情况，而是直接向服务端去确认该资源是否过期。Cache-Control中的值no-store 比较绝情，顾名思义就是不使用任何缓存策略。在 no-cache 的基础上，它连服务端的缓存确认也绕开了，只允许你直接向服务端发送请求、并下载完整的响应。



[https://www.cnblogs.com/etoah/p/5579622.html](https://www.cnblogs.com/etoah/p/5579622.html)

[https://www.cnblogs.com/etoah/p/5579622.html](https://www.cnblogs.com/etoah/p/5579622.html)

[https://www.cnblogs.com/slly/p/6732749.html](https://www.cnblogs.com/slly/p/6732749.html)

[https://www.cnblogs.com/shixiaomiao1122/p/7591556.html](https://www.cnblogs.com/shixiaomiao1122/p/7591556.html)

[https://www.cnblogs.com/etoah/p/5579622.html](https://www.cnblogs.com/etoah/p/5579622.html)

