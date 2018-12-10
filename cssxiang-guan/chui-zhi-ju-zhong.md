实现已知或者未知宽度的垂直水平居中。

```
// 1
.wraper {
  position: relative;
  .box {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 100px;
    height: 100px;
    margin: -50px 0 0 -50px;
  }
}


// 2
.wraper {
  position: relative;
  .box {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
}


// 3
.wraper {
  .box {
    display: flex;
    justify-content:center;
    align-items: center;
    height: 100px;
  }
}


// 4
.wraper {
  display: table;
  .box {
    display: table-cell;
    vertical-align: middle;
  }
}
//5
.container {
  display: grid;
  grid-auto-columns: 1fr;
  grid-auto-rows: 200px;
  background: #eee;
}
.parent {
  background: grey;
  justify-self: center; 
  align-self: center;  
}
.child {
  font-size: 30px;
}
```



