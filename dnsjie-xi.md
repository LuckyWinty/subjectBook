### DNS相关知识

DNS 是什么-- Domain Name System，域名系统，作为域名和IP地址相互映射的一个分布式数据库。

DNS Prefetching --浏览器根据自定义的规则，提前去解析后面可能用到的域名，来加速网站的访问速度。简单来讲就是提前解析域名，以免延迟。

使用方式：

```
<link rel="dns-prefetch" href="//wq.test.com">
```

**这个功能有个默认加载条件，所有的a标签的href都会自动去启用DNS Prefetching，也就是说，你网页的a标签href带的域名，是不需要在head里面加上link手动设置的。但a标签的默认启动在HTTPS不起作用。**

这时要使用 meta里面[http-equiv](https://link.jianshu.com/?t=https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta#attr-http-equiv)来强制启动功能。

```
<meta http-equiv="x-dns-prefetch-control" content="on">
```

**总结一下：**

1. DNS Prefetching是提前加载域名解析的，省去了解析时间。
2. a标签的href是可以在chrome。firefox包括高版本的IE，但是在HTTPS下面不起作用，需要meta来强制开启功能
3. 这是DNS的提前解析，并不是css，js之类的文件缓存，大家不要混淆了两个不同的概念。
4. 如果直接做了js的重定向，或者在服务端做了重定向，没有在link里面手动设置，是不起作用的。
5. 这个对于什么样的网站更有作用呢--- 类似taobao这种网站，你的网页引用了大量很多其他域名的资源，如果你的网站，基本所有的资源都在你本域名下，那么这个基本没有什么作用。因为DNS Chrome在访问你的网站就帮你缓存了。



