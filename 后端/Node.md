# Node 

fs 文件系统模块 
readFile
readFileSync
writeFile
stat

stream 支持流的模块
data 可以读取
end  流已到末尾
error 事件出错
Readable流和一个Writable流串起来  操作叫pipe

http 网络模块
request对象封装了HTTP请求
response对象封装了响应

```js
'use strict';

// 导入http模块:var http = require('http');

// 创建http server，并传入回调函数:var server = http.createServer(function (request, response) {
// 回调函数接收request和response对象,
// 获得HTTP请求的method和url:
console.log(request.method + ': ' + request.url);
// 将HTTP响应200写入response, 同时设置Content-Type: text/html:
response.writeHead(200, {'Content-Type': 'text/html'});
// 将HTTP响应的HTML内容写入response:
response.end('<h1>Hello world!</h1>');
});
```

// 让服务器监听8080端口:
server.listen(8080);

console.log('Server is running at http://127.0.0.1:8080/');

url 解析url的模块
path 处理本地文件目录模块

cypto 提供通用的加密和哈希算法

