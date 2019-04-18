### 手写call

```js
Function.prototype.myCall = function(context){
    if(typeof this !== 'function'){
        throw new TypeError('Error')
    }
    context = context || window;
    context.fn = this;
    const args = [...arguments].slice(1);
    const result = context.fn(...args);
    delete context.fn
    return result
} 
```

### 手写apply

```js
Function.prototype.myApply=function(context){
    if(typeof this !=="function"){
        throw new Error("不是函数")
    }
    const obj=context||window
    obj.fn=this
    const arg=arguments[1]||[]
    res=obj.fn(...arg)
    delete obj.fn
    return res
}
```

### 手写bind

```js
Function.prototype.myBind=function(context){
    if(typeof this !=="function"){
        throw new Error("不是函数")
    }
    const _this=this
    const arg=[...arguments].slice(1)
    return function fn(){
        if(this instanceof fn){
            return  new _this(...arg,...arguments)
        }
            return _this.call(context,...arg,...arguments)
    }
}
```

bind 相对复杂一些，主要是目标返回一个函数，所以这里要考虑三个问题
1. this的绑定问题，这里用_this=this解决
2. 由于返回的是函数，如果采取new操作符调用该如何呢？根据上一篇博客说到的是new操作符绑定this优先级最高，因此bind失效了
3. bind提供的参数要与后续调用的参数合并
