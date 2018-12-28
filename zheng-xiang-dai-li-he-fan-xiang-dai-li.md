### 概念

正向代理是一个位于客户端和目标服务器之间的代理服务器\(中间服务器\)。为了从原始服务器取得内容，客户端向代理服务器发送一个请求，并且指定目标服务器，之后代理向目标服务器转交并且将获得的内容返回给客户端。正向代理的情况下客户端必须要进行一些特别的设置才能使用。

\|客户端+代理服务器\|--&gt;\|目标服务器\|

反向代理正好相反。对于客户端来说，反向代理就好像目标服务器。并且客户端不需要进行任何设置。客户端向反向代理发送请求，接着反向代理判断请求走向何处，并将请求转交给客户端，使得这些内容就好似他自己一样，一次客户端并不会感知到反向代理后面的服务，也因此不需要客户端做任何设置，只需要把反向代理服务器当成真正的服务器就好了。

\|客户端\|--&gt;\|代理服务器+目标服务器\|

**正向代理和反向代理最关键的两点区别：**

* 是否指定目标服务器
* 客户端是否要做设置

### 使用场景

正向代理的典型用途是为在防火墙内的局域网客户端提供访问Internet的途径。正向代理还可以使用缓冲特性减少网络使用率。反向代理的典型用途是将 防火墙后面的服务器提供给Internet用户访问。反向代理还可以为后端的多台服务器提供负载平衡，或为后端较慢的服务器提供缓冲服务。

##### 正向代理

从上面的介绍也就可以猜出来正向代理的至少一个功能（俗称翻墙），也即：

用户A无法访问facebook，但是能访问服务器B，而服务器B可以访问facebook。于是用户A访问服务器B，通过服务器B去访问facebook，，服务器B收到请求后，去访问facebook，facebook把响应信息返回给服务器B，服务器B再把响应信息返回给A。这样，通过代理服务器B，就实现了翻墙。

##### 反向代理

从上面的介绍也可以猜出来反向代理的至少一个功能（比如负载均衡），也即：

假设用户A访问 [**http://www.somesite.com/something.html**](https://link.jianshu.com?t=http://www.somesite.com/something.html)，但[www.somesite.com](https://link.jianshu.com?t=http://www.somesite.com)上并不存在something.html页面，于是接收用户请求的该服务器就偷偷从另外一台服务器上取回来，然后返回给用户，而用户并不知道something.html页面究竟位于哪台机器上。

更多：[https://www.jianshu.com/p/208c02c9dd1d](https://www.jianshu.com/p/208c02c9dd1d)
