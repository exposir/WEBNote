# JS基础

### onLoad和DomContentLoad的区别

- onLoad是的在页面所有文件加载完成后执行。
- DomContentLoad是Dom加载完成后执行，不必等待样式脚本和图片加载。

>原理：如果是webkit引擎则轮询document的readyState属性，当值为loaded或者complete时则触发DOMContentLoaded事件，对webkit525之后版本直接可以注册DOMContentLoaded事件

document.readyState 属性返回当前文档的状态:
- uninitialized - 还未开始载入
- loading - 载入中
- interactive - 已加载，文档与用户可以开始交互   
- complete - 载入完成

### this

![this](../assets/this_js.png)

