## ng基本知识

Nginx 是一个高性能的 HTTP 和反向代理服务器，是一款轻量级的 Web 服务器，可以实现负载均衡等功能。其优点是可以实现高并发、部署简单、内存消耗少、成本低等。主要缺点是rewrite功能不够强大，模块没有Apache多。

SSI

SSI：Server Side Include，是一种基于服务端的网页制作技术，大多数（尤其是基于Unix平台）的web服务器如Netscape Enterprise Server等均支持SSI命令。

它的工作原因是：在页面内容发送到客户端之前，使用SSI指令将文本、图片或代码信息包含到网页中。对于在多个文件中重复出现内容，使用SSI是一种简便的方法，将内容存入一个包含文件中即可，不必将其输入所有文件。通过一个非常简单的语句即可调用包含文件，此语句指示 Web 服务器将内容插入适当网页。而且，使用包含文件时，对内容的所有更改只需在一个地方就能完成。

### 在Nginx配置中开启SSI

```
    server {  
        listen  10.3.9.27:80;  
        server_name  www.test.me;  
        location / {  
            ssi on;  
            ssi_silent_errors on;  
            ssi_types text/shtml;  
            index index.shtml;  
            root /usr/local/web/wwwroot;  
            expires 30d;  
            access_log      /data/logs/www.ball.com-access_log main;  
        }  
    }
```



