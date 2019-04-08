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

