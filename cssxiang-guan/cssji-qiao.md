##### 图片自适应占位方式

对于图片高度自适应的情况，

当图片未正确加载，或加载完成前，由于图片高度为0，其容器会因为没有内容，导致容器无法撑高而塌陷，而如果加载较慢则会再图片加载完成后出现闪烁的情况。css中，当padding-top/bottom值为百分比时，其值都是以其父元素的宽度为参照对象。因此

对于宽高比例固定的情况，可以利用padding-top/bottom用于图片自适应占位，解决页面闪烁的问题。具体参考：

[https://www.jianshu.com/p/65887e7e649d](https://www.jianshu.com/p/65887e7e649d)。

但是对于宽高比例不定的图片来说，这样做可能导致图片显示不全，使用时要注意。

css画特殊图形

扇形：

```
    width: 0;
    height: 0;
    border: solid 100px red;
    border-color: red transparent transparent transparent;
    border-radius: 100px;
```



