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
    fs.readFile('input.txt', function(err, data) {
        if(err) reject(err);
        resolve(data);
    });
})
p1.then(r1=>console.log(r1.toString())).catch(e1=>console.log(e1));
```
上面的例子如果读文件的异步操作fs.readFile成功了就通过resolve函数返回读出的data数据，并在then中成功时执行的函数进行打印输出。
一般不用then里作为第二个参数的函数来处理对应reject函数返回的err，一般所有的错误都可以通过catch来捕捉。

好解释 =》https://blog.csdn.net/xiubinxu/article/details/113250738

## 2. 网站登录逻辑的进化 http=>cookie=>session=>JWT token

http是无状态服务，每次客户端向服务器都要重新发送用户名和密码。

cookie, 服务器校验过客户端第一次发过来的账号密码之后，把账号密码信息返还给浏览器，以key、value的形式存在浏览器的cookie里，下次用户不用再输入用户名和密码，浏览器会自动将cookie里的信息包含在请求头中发给服务器，用户名和密码明文存在cookie中不安全。

session，为了解决cookie的安全问题，会在服务器端为每个登录客户端创建一个session，把客户的账号信息存在服务器session端，把session id返回给浏览器客户端，浏览器下次访问服务器只需要利用存在cookie中的session id就可告知服务器登录用户是谁。海量用户时，session会给服务器造成负荷。

JWT token, JWT规定了数据传输的结构，一串完整的JWT由三段落组成，每个段落用英文句号连接（.）连接，他们分别是：Header、Payload、Signature，所以，常规的JWT内容格式是这样的：AAA.BBB.CCC
Header包含加密的方式、type, payload包含实际传递的参数内容,signature主要决定了Header、Payload有没有被人篡改

