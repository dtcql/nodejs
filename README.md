# NodeJs学习笔记

## 1. 异步函数，回调函数，Promise

### 1.1 异步函数

有些函数很耗时，比如等待几秒或者去服务器取数据，这时候在主线程的所有同步执行完成后，启动一些子线程去执行每个异步操作，从而节省时间。

### 1.2 回调函数

当异步操作完成后，仍需要进行一些该异步操作的后续处理的时候，要把后续处理写成一个回调函数，把回调函数的函数名称或者函数本身当作异步操作函数的参数，
在异步操作完成后会调用这个回调函数，从而实现了异步操作完成后再去调用回调函数中的这些后续处理。
```
var fs = require("fs");

fs.readFile('input.txt', <font color=red> function (err, data) {
    if (err) return console.error(err);
    console.log(data.toString());</font>
});

console.log("程序执行结束!");
```

### 1.3 Promise


