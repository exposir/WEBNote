# Koa

`http://www.ruanyifeng.com/blog/2017/08/koa.html`

### Context对象 

表示一次对话的上下文（包括HTTP请求和HTTP回复）

ctx.response.body 属性就是发送给用户的内容

ctx.request.accepts 判断数据类型

ctx.reponse.type 指定返回类型

### 路由
ctx.request.path 可以获取用户请求的路径

koa-router 模块

app.use(route.get('/',maxin));

koa-static 静态资源

ctx.response.redirect() 302重定向

### 中间件(middleware)

```js
const logger = (ctx, next) => {
  console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
  next();
}
app.use(logger);
```

looger函数就叫做中间件，因为它处在HTTP Request和HTTP Response中间，用来实现某种中间功能。

app.use()用来加载中间件。

中间件栈：多个中间件会形成一个栈结构，以“先进后出”的顺序执行。

1. 最外层的中间件首先执行。
2. 调用next函数，把执行权交给下一个中间件。
3. ...
4. 最内层的中间件最后执行。
5. 执行结束后，把执行权交回上一层的中间件。
6. ...
7. 最外层的中间件收回执行权之后，执行next函数后面的代码。

```js
const main = async function (ctx, next) {
  ctx.response.type = 'html';
  ctx.response.body = await fs.readFile('./demos/template.html', 'utf8');
};
```

fs.readFile是一个异步操作，必须写成await fs.readFile(),然后中间件必须写成async函数。

koa-compose 可以将多个中间件合成为一个。

```js
const compose = require('koa-compose');
const middlewares = compose([logger, main]);
app.use(middlewares);
```

### 错误处理

ctx.throw(500) 抛出500错误

返回404错误
```js
const main = ctx => {
  ctx.response.status = 404;
  ctx.response.body = 'Page Not Found';
};
```

try...catch 处理错误的中间件

error 事件的监听 

```js
app.on('error', (err, ctx) => {
  console.error('server error', err);
});
```

ctx.app.emit() 释放error事件

手动释放被try...catch捕获的error事件

### Web App 的功能

ctx.cookies 用来读写Coookie

koa-body 模块可以用来从POST请求的数据体里面提取键值对。

boa-body 模块还可以用来处理文件上传。



