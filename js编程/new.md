### new

```js
    function car(id) {
      this.id = id;
      this.age = 18;
    }

//new的实际过程，创建临时对象，绑定原型，return回临时对象。
    function car(id) {
      var temp = {};
      temp.__proto__ = car.prototype;
      temp.id = id;
      temp.age = 18;
      return temp
    }

    var bus = new car(30)
    console.log(bus.id) //30
```
