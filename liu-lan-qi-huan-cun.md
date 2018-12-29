## HTTP缓存

HTTP 缓存是我们日常开发中最为熟悉的一种缓存机制。它又分为**强缓存**和**协商缓存**。优先级较高的是强缓存，在命中强缓存失败的情况下，才会走协商缓存。

### 强缓存

强缓存是利用 http 头中的 Expires 和 Cache-Control 两个字段来控制的。强缓存中，当请求再次发出时，浏览器会根据其中的 expires 和 cache-control 判断目标资源是否“命中”强缓存，若命中则直接从缓存中获取资源，**不会再与服务端发生通信。**

### 强缓存的实现：从 expires 到 cache-control

实现强缓存，过去我们一直用`expires`。当服务器返回响应时，在 Response Headers 中将过期时间写入 expires 字段。像这样

```
expires: Wed, 12 Sep 2019 06:12:18 GMT
```

可以看到，expires 是一个时间戳，接下来如果我们试图再次向服务器请求资源，浏览器就会先对比本地时间和 expires 的时间戳，如果本地时间小于 expires 设定的过期时间，那么就直接去缓存中取这个资源。

从这样的描述中大家也不难猜测，expires 是有问题的，它最大的问题在于对“本地时间”的依赖。如果服务端和客户端的时间设置可能不同，或者我直接手动去把客户端的时间改掉，那么 expires 将无法达到我们的预期。

考虑到 expires 的局限性，HTTP1.1 新增了`Cache-Control`字段来完成 expires 的任务。expires 能做的事情，Cache-Control 都能做；expires 完成不了的事情，Cache-Control 也能做。因此，Cache-Control 可以视作是 expires 的**完全替代方案**。在当下的前端实践里，我们继续使用 expires 的唯一目的就是**向下兼容**。

cache-control:

```
cache-control: max-age=31536000
```

在 Cache-Control 中，我们通过`max-age`来控制资源的有效期。max-age 不是一个时间戳，而是一个时间长度。在本例中，max-age 是 31536000 秒，它意味着该资源在 31536000 秒以内都是有效的，完美地规避了时间戳带来的潜在问题。

**Cache-Control 相对于 expires 更加准确，它的优先级也更高。当 Cache-Control 与 expires 同时出现时，我们以 Cache-Control 为准。**

### 协商缓存：浏览器与服务器合作之下的缓存策略

协商缓存依赖于服务端与浏览器之间的通信。协商缓存机制下，浏览器需要向服务器去询问缓存的相关信息，进而判断是重新发起请求、下载完整的响应，还是从本地获取缓存的资源。如果服务端提示缓存资源未改动（Not Modified），资源会被**重定向**到浏览器缓存，**这种情况下网络请求对应的状态码是 304**。

### 协商缓存的实现：从 Last-Modified 到 Etag

Last-Modified 是一个时间戳，如果我们启用了协商缓存，它会在首次请求时随着 Response Headers 返回：

```
Last-Modified: Fri, 27 Oct 2017 06:35:57 GMT
```

随后我们每次请求时，会带上一个叫 If-Modified-Since 的时间戳字段，它的值正是上一次 response 返回给它的 last-modified 值：

```
If-Modified-Since: Fri, 27 Oct 2017 06:35:57 GMT
```

服务器接收到这个时间戳后，会比对该时间戳和资源在服务器上的最后修改时间是否一致，从而判断资源是否发生了变化。如果发生了变化，就会返回一个完整的响应内容，并在 Response Headers 中添加新的 Last-Modified 值；否则，返回如上图的 304 响应，Response Headers 不会再添加 Last-Modified 字段。

**使用 Last-Modified 存在一些弊端，这其中最常见的就是这样两个场景：**

* 我们编辑了文件，但文件的内容没有改变。服务端并不清楚我们是否真正改变了文件，它仍然通过最后编辑时间进行判断。因此这个资源在再次被请求时，会被当做新资源，进而引发一次完整的响应——不该重新请求的时候，也会重新请求。
* 当我们修改文件的速度过快时（比如花了 100ms 完成了改动），由于 If-Modified-Since 只能检查到以秒为最小计量单位的时间差，所以它是感知不到这个改动的——该重新请求的时候，反而没有重新请求了。

这两个场景其实指向了同一个 bug——服务器并没有正确感知文件的变化。为了解决这样的问题，Etag 作为 Last-Modified 的补充出现了。

Etag 是由服务器为每个资源生成的唯一的**标识字符串**，这个标识字符串可以是基于文件内容编码的，只要文件内容不同，它们对应的 Etag 就是不同的，反之亦然。因此 Etag 能够精准地感知文件的变化。

Etag 的生成过程需要服务器额外付出开销，会影响服务端的性能，这是它的弊端。因此启用 Etag 需要我们审时度势。正如我们刚刚所提到的——Etag 并不能替代 Last-Modified，它只能作为 Last-Modified 的补充和强化存在。

**Etag 在感知文件变化上比 Last-Modified 更加准确，优先级也更高。当 Etag 和 Last-Modified 同时存在时，以 Etag 为准。**

![](/assets/webcache.png)

更多：[https://juejin.im/book/5b936540f265da0a9624b04b/section/5b9ba651f265da0ac726e5de\(小册：\[前端性能优化原理与实践\]\(https://juejin.im/book/5b936540f265da0a9624b04b\)\](https://juejin.im/book/5b936540f265da0a9624b04b/section/5b9ba651f265da0ac726e5de%28小册：[前端性能优化原理与实践]%28https://juejin.im/book/5b936540f265da0a9624b04b%29\)\)

**ETag**

ETag：是实体标签\(Entity Tag\)的缩写。ETag一般不以明文形式相应给客户端。在资源的各个生命周期中，它都具有不

同的值，用于标识出资源的状态。当资源发生变更时，如果其头信息中一个或者多个发生变化，或者消息实体发生变化

，那么ETag也随之发生变化。

ETag值的变更说明资源状态已经被修改。往往可以通过时间戳就可以便宜的得到ETag头信息。在服务端中如果发回给

消费者的相应从一开始起就由ETag控制，那么可以确保更细粒度的ETag升级完全由服务来进行控制。服务计算ETag值，

并在相应客户端请求时将它返回给客户端。

**计算ETag值**

在HTTP1.1协议中并没有规范如何计算ETag。ETag值可以是唯一标识资源的任何东西，如持久化存储中的某个资源关联

的版本、一个或者多个文件属性，实体头信息和校验值、\(CheckSum\)，也可以计算实体信息的散列值。有时候，为了计

算一个ETag值可能有比较大的代价，此时可以采用生成唯一值等方式\(如常见的GUID\)。无论怎样，服务都应该尽可能的

将ETag值返回给客户端。计算ETag值开销最大的一般是计算采用哈希算法获取资源的表述值。

更多：[https://www.cnblogs.com/tyb1222/archive/2011/12/24/2300246.html](https://www.cnblogs.com/tyb1222/archive/2011/12/24/2300246.html)



## Service Worker Cache

Service Worker 是一种独立于主线程之外的 Javascript 线程。它脱离于浏览器窗体，因此无法直接访问 DOM。这样独立的个性使得 Service Worker 的“个人行为”无法干扰页面的性能，这个“幕后工作者”可以帮我们实现离线缓存、消息推送和网络代理等功能。我们借助 Service worker 实现的离线缓存就称为 Service Worker Cache。

Service Worker 的生命周期包括 install、active、working 三个阶段。一旦 Service Worker 被 install，它将始终存在，只会在 active 与 working 之间切换，除非我们主动终止它。这是它可以用来实现离线存储的重要先决条件。

## MemoryCache

MemoryCache，是指存在内存中的缓存。从优先级上来说，它是浏览器最先尝试去命中的一种缓存。从效率上来说，它是响应速度最快的一种缓存。

内存缓存是快的，也是“短命”的。它和渲染进程“生死相依”，当进程结束后，也就是 tab 关闭以后，内存里的数据也将不复存在



