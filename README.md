# NodeJs学习笔记

## 1. 异步函数，回调函数，Promise

### 1.1 异步函数

有些函数很耗时，比如等待几秒或者去服务器取数据，这时候在主线程的所有同步执行完成后，启动一些子线程去执行每个异步操作，从而节省时间。

### 1.2 回调函数

当异步操作完成后，仍需要进行一些该异步操作的后续处理的时候，要把后续处理写成一个回调函数，把回调函数的函数名称或者函数本身当作异步操作函数的参数，
在异步操作完成后会调用这个回调函数，从而实现了异步操作完成后再去调用回调函数中的这些后续处理。
```
var fs = require("fs");

fs.readFile('input.txt', function (err, data) {
    if (err) return console.error(err);
    console.log(data.toString());
});

console.log("程序执行结束!");
```
回调函数一般作为异步函数的最后一个参数，例如
```
function (err, data) {
    if (err) return console.error(err);
    console.log(data.toString());
}
```
部分就是异步函数fs.readFile的回调函数，读文件结束之后，调用回调函数输出。


### 1.3 构造函数Promise

调用一层回调函数就有些复杂了，如果调用多层回调函数就不太容易理解了，ES6新加了Promise的概念，Promise的一个构造函数，Promise的中文意思是承诺。
Promise会把每一个异步操作封装成一个承诺，这个承诺/异步函数成功时调用异步操作的后续处理，失败时调用错误处理。避免嵌套回调的回调地狱问题。

```
const fs = require("fs");
p1 = new Promise( (resolve, reject) => {
    fs.readFile('input.txt', function (err, data) {
        if (err) reject(err);
        resolve(data);
    });
})
p1.then(r1=>console.log(r1.toString())).catch(e1=>console.log(e1));
```
上面的例子如果读文件的异步操作fs.readFile成功了就通过resolve函数返回读出的data数据，并在then中成功时执行的函数进行打印输出，所有的错误都可以通过catch来捕捉。