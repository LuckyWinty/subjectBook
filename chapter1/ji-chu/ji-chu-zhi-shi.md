1、对于原始类型来说，你想直接通过`instanceof`来判断类型是不行的，当然我们还是有办法让`instanceof`判断原始类型的：

```
class PrimitiveString {
  static [Symbol.hasInstance](x) {
    return typeof x === 'string'
  }
}
console.log('hello world' instanceof PrimitiveString) // true
```

`Symbol.hasInstance`，其实就是一个能让我们自定义`instanceof`行为的东西，以上代码等同于`typeof 'hello world' === 'string'`，所以结果自然是`true`了。这其实也侧面反映了一个问题，`instanceof`也不是百分之百可信的。

