
###XMLHttpRequest

使用 **XMLHttpRequest**（XHR）对象可以与服务器交互。您可以从URL获取数据，而无需让整个的页面刷新。这允许网页在不影响用户的操作的情况下更新页面的局部内容。在 AJAX 编程中，**XMLHttpRequest** 被大量使用。

```
EventTarget <-- XMLHttpRequestEventTarget <-- XMLHttpReauest
```

尽管名称如此，XMLHttpRequest 可以用于获取任何类型的数据，而不仅仅是XML，它甚至支持 HTTP 以外的协议（包括 file:// 和 FTP）。

如果您的通信流程需要从服务器接收事件或消息数据，请考虑通过 EventSource 接口使用 server-sent events。对于全双工的通信， WebSocket 则可能是更好的选择。

####属性

`XMLHttpRequest.onreadystatechange`
当 readyState 属性发生变化时调用的 EventHandler。

`XMLHttpRequest.readyState` 只读
返回 一个无符号短整型（unsigned short）数字，代表请求的状态码。

`XMLHttpRequest.response` 只读
返回一个 ArrayBuffer、Blob、Document，或 DOMString，具体是哪种类型取决于 XMLHttpRequest.responseType 的值。其中包含整个响应体（response body）。

`XMLHttpRequest.status` 只读
返回一个无符号短整型（unsigned short）数字，代表请求的响应状态

#####事件处理球

作为 XMLHttpRequest 实例的属性，所有浏览器都支持 onreadystatechange。

后来，许多浏览器实现了一些额外的事件（onload、onerror、onprogress 等）。详见Using XMLHttpRequest。

更多现代浏览器，包括 Firefox，除了可以设置 on* 属性外，也提供标准监听器 addEventListener() API 来监听XMLHttpRequest事件。

####方法

`XMLHttpRequest.abort()`
如果请求已被发送，则立刻中止请求。

`XMLHttpRequest.open()`
初始化一个请求。该方法只能在 JavaScript 代码中使用，若要在 native code 中初始化请求，请使用 openRequest()。

`XMLHttpRequest.send()`
发送请求。如果请求是异步的（默认），那么该方法将在请求发送后立即返回。

`XMLHttpRequest.setRequestHeader()`
设置 HTTP 请求头的值。您必须在 open() 之后、send() 之前调用 setRequestHeader() 方法。

####事件

`abort`
当 request 被停止时触发，例如当程序调用 XMLHttpRequest.abort() 时。
也可以使用 onabort 属性。

`error`
当request遭遇错误时触发。
也可以使用 onerror 属性。

`load`
XMLHttpRequest请求成功完成时触发。
也可以使用 onload 属性.
