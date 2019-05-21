# Koa

### Context对象 

表示一次对话的上下文（包括HTTP请求和HTTP回复）

ctx.response.body 属性就是发送给用户的内容

ctx.request.accepts 判断数据类型

ctx.reponse.type 指定返回类型

### 路由
ctx.request.path 可以获取用户请求的路径

koa-router模块

app.use(route.get('/',maxin));

