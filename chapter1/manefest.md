manifest文件，基本格式为三段：

```
CACHE， 
NETWORK，
FALLBACK
```

CACHE:（必须）标识出哪些文件需要缓存，可以是相对路径也可以是绝对路径。

NETWORK:（可选）这一部分是要绕过缓存直接读取的文件，可以使用通配符＊。

FALLBACK：指定了一个后备页面，当资源无法访问时，浏览器会使用该页面。该段落的每条记录都列出两个 URI。第一个表示资源， 第二个表示后备页面。

其中NETWORK和FALLBACK为可选项。

**而第一行CACHE MANIFEST为固定格式，必须写在前面。**

以\#号开头的是注释，一般会在第二行写个版本号，用来在缓存的文件更新时，更改manifest的作用，可以是版本号，时间戳或者md5码等等。

典型结构：

```
CACHE MANIFEST
#version 1.2.2

CACHE:
#css
http://www.haorooms.com/theme/assets/style.css
#js
http://www.haorooms.com/theme/assets/js/main.js

#img
http://static.hyb.dev.ipo.com/css/wifi/pc/images/logo-fk1.png
http://static.hyb.dev.ipo.com/css/wifi/images/favicon.ico

NETWORK:  
*
FALLBACK:
 /404.html
```



