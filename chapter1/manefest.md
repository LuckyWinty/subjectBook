manifest文件，基本格式为三段：

```
CACHE， 
NETWORK，
FALLBACK
```

其中NETWORK和FALLBACK为可选项。

**而第一行CACHE MANIFEST为固定格式，必须写在前面。**

以\#号开头的是注释，一般会在第二行写个版本号，用来在缓存的文件更新时，更改manifest的作用，可以是版本号，时间戳或者md5码等等。

