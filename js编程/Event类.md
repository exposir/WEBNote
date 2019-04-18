要求可以实现链式操作，具有on，off, emit,once
```js
class EventEmitter{
    constructor(){
        this._events={}
    }
    on(event,callback){
        let callbacks = this._events[event] || []
        callbacks.push(callback)
        this._events[event] = callbacks
        return this
    }
    off(event,callback){
        let callbacks = this._events[event]
        this._events[event] = callbacks && callbacks.filter(fn => fn !== callback)
        return this
    }
    emit(...args){
        const event = args[0]
        const params = [].slice.call(args,1)
        const callbacks = this._events[event]
        callbacks.forEach(fn => fn.apply(this, params))
        return this
    }
    once(event,callback){
        let wrapFunc = (...args) => {
            callback.apply(this,args)
            this.off(event,wrapFunc)
        }
        this.on(event,wrapFunc)
        return this
    }
}

// 测试
let ee = new EventEmitter();
function a() {
    console.log('a')
}
function b() {
    console.log('b')
}
function c() {
    console.log('c')
}
function d(...a) {
    console.log('d',...a)
}
ee.on('TEST1', a).on('TEST2', b).once('TEST2', c).on('TEST2',d);

ee.emit('TEST1');
console.log('....')
ee.emit('TEST2');
// In test2
// In test2 again
console.log('....')
ee.emit('TEST2');
```