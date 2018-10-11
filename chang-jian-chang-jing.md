1、根据name取cookie中的值

```
function get(name){
    var reg = new RegExp(name+'=([^;]*)?s(;|$)');
    var res = reg.exec(document.cookie);
    if(!res || !res[1])return '';
    try{
        if(/(%[0-9A-F]{2}){2,}/.test(res)){//utf8编码
           return decodeURIComponent(res);
        }else{//unicode编码
            return unescape(res);
        }            
    }catch(e){
        return unescape(res);
    }
}
```

正则表达式中重点看这几句代码:`'([^;]*)'`, 意思是匹配`str=`后面的不为`;`\(`[^;]`表示非集, 也就是所有不为`;`的字符都能被匹配\)的字符串, 该字符串出现0或更多次\(`*`\), 之后将匹配到的字符串放入第一个捕获组.

