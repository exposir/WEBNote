### this指向是真他娘的烦人啊！

#### 第一题

```js
    var name = 'windowName';
    var person = {
      name: 'personName',
      pro: {
        name: 'proName',
        getName: function () {
          return this.name;
        }
      }
    }
    console.log(person.pro.getName());
    var people = person.pro.getName;
    console.log(people())
```
#### 第二题

```js
var name = 'windowName';
var a ={
  fn:function(){
    console.log(this.name)
  }
}
window.a.fn();
```

#### 第三题 

```js
var name = 'windowName';
function fn(){
  innerFunction()
  function innerFunction(){
    console.log(this.name)
  }
}
fn()
```

#### 第四题 ⭐

```js
var name = 'windowName';
var a = {
  name:'menglingyu',
  fn:function(a,b){
    console.log(a+b)
    console.log(this.name)
    console.log(name)
  }
}
a.fn(1,2);
var b = a.fn;
b(1,2)
b.call(a,1,2)
b.apply(a,1,2)
b.bind(a,1,2)()
```

#### 第五题 奇葩

5.1

```js
    let a = 10;
    var b = 20;
    var c = {
      a: 30,
      o: function () {
        setTimeout(function () {
          console.log(this.a)
          console.log(this.b)
        }, 1000)
      }
    }
    c.o()
```
5.2

```js
    let a = 10;
    var b = 20;
    var c = {
      a: 30,
      o: function () {
        setTimeout(()=> {
          console.log(this.a)
          console.log(this.b)
        }, 1000)
      }
    }
    c.o()
```

5.3

```js
    let a = 10;
    var b = 20;
    var c = {
      a: 30,
      o: ()=> {
        setTimeout(()=> {
          console.log(this.a)
          console.log(this.b)
        }, 1000)
      }
    }
    c.o()
```

5.4

```js
    let a = 10;
    var b = 20;
    var c = {
      a: 30,
      o: ()=> {
        setTimeout(function() {
          console.log(this.a)
          console.log(this.b)
        }, 1000)
      }
    }
    c.o()
```

#### 第六题 伴鱼⭐

```js
        var name = "winow";
        function func() {
            console.log(this.name);
            console.log(this)
        }
        var obj = {
            name: "object",
            getNameFunc: function (fn) {
                fn && fn();
                return function () {
                    return this.name;
                }
            }
        }
        console.log(obj.getNameFunc(func)());

        //window window
```

### 定时器真有意思啊！

####第一题

```js
    var i = 0;
    var j = 0;
    while (i < 3) {
      timer = window.setTimeout(function () {
        i++;
        j++;
        console.log(j)
      }, 0)
    }
    clearTimeout(timer)
    //卡死

    var i = 0;
    var j = 0;
    while (i++ < 3) {
      timer = window.setTimeout(function () {
        j++;
        console.log(j)
      }, 0)
    }
    clearTimeout(timer)
    //1,2
```
### 实现一个Person
```js
Person("Smith").sleepFirst(5).eat("supper");
// 输出：
// 等待5秒
// Wake up after 5
// Hi This is Smith!
// Eat supper

Person("Dan").sleep(10).eat("dinner");
// 输出：
// Hi! This is Dan!
// 等待10秒..
// Wake up after 10
// Eat dinner~

        function Person(name) {
            if (!(this instanceof Person)) {
                return new Person(name)
            }
            this.tasks = [];
            var self = this;
            var fn = function () {
                console.log('Hi This is name ' + '!');
                self.next();
            }
            this.tasks.push(fn);
            setTimeout(function () {
                self.next();
            }, 0);
        }
        Person.prototype.next = function () {
            var fn = this.tasks.shift();
            fn && fn();
        }
        Person.prototype.eat = function (name) {
            var self = this;
            var fn = function () {
                console.log('Eat ' + name + '~');
                self.next();
            }
            this.tasks.push(fn);
            return this;
        }
        Person.prototype.sleep = function (time) {
            var self = this;
            var fn = (function (time) {
                return function () {
                    setTimeout(function () {
                        console.log("Wake up after " + time);
                        self.next();
                    }, time * 1000);
                }
            })(time);
            this.tasks.push(fn);
            return this;
        }
        Person.prototype.sleepFirst = function (time) {
            var self = this;
            var fn = (function (time) {
                return function () {
                    setTimeout(function () {
                        console.log("Wake up after " + time);
                        self.next();
                    }, time * 1000);
                }
            })(time);
            this.tasks.unshift(fn);
            return this;
        }

        Person("Smith").sleepFirst(1).eat("supper").sleep(2).eat("supper")
```
### 对象复制

```js
        var obj1 = { a: 11 };
        function fn(obj) {
            obj.a = 22;
            obj = { a: 33 };
            obj.b = 44;
        }
        fn(obj1);
        console.log(obj1);
```

### 变量声明 水滴互助

```js
        var a = 10;
        function test() {
            var a = 20;
            return function () {
                a = 30
            }
        }
        var fn = test();
        fn()
```

### 函数声明 水滴互助

```js
        function test() {
            return func();
            var func = function () {
                return a = 10
            }
            function func() {
                return a = 20
            }
            var func = (function () {
                return a = 30;
            })

        }
```