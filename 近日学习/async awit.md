async function 用来定义一个返回 AsyncFunction 对象的异步函数。异步函数是指通过事件循环异步执行的函数，它会通过一个隐式的 Promise 返回其结果。如果你在代码中使用了异步函数，就会发现它的语法和结构会更像是标准的同步函数。

你还可以使用 异步函数表达式 来定义异步函数。

```js
function resolveAfter2Seconds() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve("resolved");
    }, 2000);
  });
}

async function asyncCall() {
  console.log("calling");
  var result = await resolveAfter2Seconds();
  console.log(result);
  // expected output: 'resolved'
}

asyncCall(); // calling resolved
```

语法

`async function name([param[, param[, ... param]]]) { statements }`

参数

name 函数名称
param 参数
statements 函数体语句

返回值

返回的**Promise**对象会运行执行(resolve)异步函数的返回结果，或者运行拒绝(reject)——如果异步函数抛出异常的话。

描述

异步函数可以包含**await**指令，该指令会暂停异步函数的执行，并等待Promise执行，然后继续执行异步函数，并返回结果。

记住，await 关键字只在异步函数内有效。如果你在异步函数外使用它，会抛出语法错误。

注意，当异步函数暂停时，它调用的函数会继续执行(收到异步函数返回的隐式Promise)

`async/await的目的是简化使用多个 promise 时的同步行为，并对一组 Promises执行某些操作。正如Promises类似于结构化回调，async/await更像结合了generators和 promises。`






