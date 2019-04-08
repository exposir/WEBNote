### 浏览器
#### 进程和线程
- 进程是cpu资源分配的最小单位（是能拥有资源和独立运行的最小单位）
- 线程是cpu调度的最小单位（线程是建立在进程的基础上的一次程序运行单位，一个进程中可以有多个线程）
- 不同进程之间也可以通信，不过代价较大
- 现在，一般通用的叫法：单线程与多线程，都是指在一个进程内的单和多。（所以核心还是得属于一个进程才行）

#### 浏览器都包含哪些进程？
- Browser进程
浏览器的主进程（负责协调、主控），只有一个。作用有负责浏览器界面显示，与用户交互。如前进，后退等负责各个页面的管理，创建和销毁其他进程将Renderer进程得到的内存中的Bitmap，绘制到用户界面上网络资源的管理，下载等第
- 三方插件进程
每种类型的插件对应一个进程，仅当使用该插件时才创建
- GPU进程
最多一个，用于3D绘制等。
- 浏览器渲染进程（浏览器内核）（Renderer进程，内部是多线程的）
默认每个Tab页面一个进程，互不影响。主要作用为页面渲染，脚本执行，事件处理等。

      在浏览器中打开一个网页相当于新起了一个进程（进程内有自己的多线程）


#### 浏览器内核（渲染进程）包含哪些线程？   

##### GUI渲染线程
- 负责渲染浏览器界面，解析HTML，CSS，构建DOM树和RenderObject树，布局和绘制等。
- 当界面需要重绘（Repaint）或由于某种操作引发回流(reflow)时，该线程就会执行
- 注意，GUI渲染线程与**JS引擎线程是互斥的**，当JS引擎执行时GUI线程会被挂起（相当于被冻结了），GUI更新会被保存在一个队列中**等到JS引擎空闲时**立即被执行。
##### JS引擎线程
- 也称为JS内核，负责处理Javascript脚本程序。（例如V8引擎）
- JS引擎线程负责解析Javascript脚本，运行代码。
- JS引擎一直等待着任务队列中任务的到来，然后加以处理，一个Tab页（renderer进程）中无论什么时候都只有一个JS线程在运行JS程序
- 同样注意，GUI渲染线程与JS引擎线程是互斥的，所以如果JS执行的时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞。
##### 事件触发线程
- 归属于浏览器而不是JS引擎，用来控制事件循环（可以理解，JS引擎自己都忙不过来，需要浏览器另开线程协助）
- 当JS引擎执行代码块如setTimeOut时（也可来自浏览器内核的其他线程,如鼠标点击、AJAX异步请求等），会将对应任务添加到事件线程中
- 当对应的事件符合触发条件被触发时，该线程会把事件添加到待处理队列的队尾，等待JS引擎的处理
- 注意，由于JS的单线程关系，所以这些待处理队列中的事件都得排队等待JS引擎处理（当JS引擎空闲时才会去执行）
##### 定时触发器线程
- 传说中的setInterval与setTimeout所在线程
- 浏览器定时计数器并不是由JavaScript引擎计数的,（因为JavaScript引擎是单线程的, 如果处于阻塞线程状态就会影响记计时的准确）
- 因此通过单独线程来计时并触发定时（计时完毕后，添加到事件队列中，等待JS引擎空闲后执行）
- 注意，W3C在HTML标准中规定，规定要求setTimeout中低于4ms的时间间隔算为4ms。
##### 异步http请求线程
- 在XMLHttpRequest在连接后是通过浏览器新开一个线程请求
- 将检测到状态变更时，如果设置有回调函数，异步线程就产生状态变更事件，将这个回调再放入事件队列中。再由JavaScript引擎执行。

![process](../assets/process.jpg)


####  Browser进程和浏览器内核（Renderer进程）的通信过程


![process2](../assets/process2.jpg)


####  GUI渲染线程与JS引擎线程互斥

由于JavaScript是可操纵DOM的，如果在修改这些元素属性同时渲染界面（即JS线程和UI线程同时运行），那么渲染线程前后获得的元素数据就可能不一致了。因此为了防止渲染出现不可预期的结果，浏览器设置GUI渲染线程与JS引擎为互斥的关系，当JS引擎执行时GUI线程会被挂起，GUI更新则会被保存在一个队列中等到JS引擎线程空闲时立即被执行。

####  JS阻塞页面加载

从上述的互斥关系，可以推导出，JS如果执行时间过长就会阻塞页面。譬如，假设JS引擎正在进行巨量的计算，此时就算GUI有更新，也会被保存到队列中，等待JS引擎空闲后执行。然后，由于巨量计算，所以JS引擎很可能很久很久后才能空闲，自然会感觉到巨卡无比。所以，要尽量避免JS执行时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞的感觉。

####  简单梳理下浏览器渲染流程

1.解析html建立dom树

2.解析css构建render树（将CSS代码解析成树形的数据结构，然后结合DOM合并成render树）

3.布局render树（Layout/reflow），负责各元素尺寸、位置的计算

4.绘制render树（paint），绘制页面像素信息

5.浏览器会将各层的信息发送给GPU，GPU会将各层合成（composite），显示在屏幕上。

####  load事件与DOMContentLoaded事件的先后

- 当 DOMContentLoaded 事件触发时，仅当DOM加载完成，不包括样式表，图片。(譬如如果有async加载的脚本就不一定完成)
- 当 onload 事件触发时，页面上所有的DOM，样式表，脚本，图片都已经加载完成了。（渲染完毕了）

所以，顺序是：`DOMContentLoader -> load`

####  css加载是否会阻塞dom树渲染？

这里说的是头部引入css的情况

首先，我们都知道：**css是由单独的下载线程异步下载的。**
- css加载不会阻塞DOM树解析（异步加载时DOM照常构建）
- 但会阻塞render树渲染（渲染时需等css加载完毕，因为render树需要css信息）

### Event Loop
#### 从Event loop 谈JS的运行机制
- JS分为同步任务和异步任务
- 同步任务都在主线程上执行，形成一个`执行栈`
- 主线程之外，事件触发线程管理着一个`任务队列`，只要异步任务有了运行结果，就在`任务队列`之中放置一个事件。
- 一旦执行栈中的所有同步任务执行完毕（此时JS引擎空闲），系统就会读取任务队列，将可运行的异步任务添加到可执行栈中，开始执行。

![process2](../assets/eventloop.jpg)

>为什么有时候setTimeout推入的事件不能准时执行？
因为可能在它推入到事件列表时，主线程还不空闲，正在执行其它代码，

#### 补充

![process2](../assets/eventloop2.jpg)

- 主线程运行时会产生执行栈，栈中的代码调用某些api时，它们会在事件队列中添加各种事件（当满足触发条件后，如ajax请求完毕）
- 而栈中的代码执行完毕，就会读取事件队列中的事件，去执行那些回调，如此循环。
- 注意，总是要等待栈中的代码执行完毕后才会去读取事件队列中的事件

#### 定时器

什么时候会用到定时器线程？当使用`setTimeout`或`setInterval`时，它需要定时器线程计时，计时完成后就会将特定的事件推入事件队列中。

```js
setTimeout(function(){
    console.log('hello!');
}, 0);

console.log('begin');
```
- 执行结果：`bigin hello`
- W3C在HTML标准中规定，要求setTimeout中低于4ms的时间间隔就算为4ms。
- 假设0ms就推入时间队列，也会先执行`bigin`，因为只有可执行栈内空了后才会主动读取事件队列。

#### macrotask与microtask

```js
console.log('script start');

setTimeout(function() {
    console.log('setTimeout');
}, 0);

Promise.resolve().then(function() {
    console.log('promise1');
}).then(function() {
    console.log('promise2');
});

console.log('script end');
```

他的执行结果是：

```js
script start  
script end   //第一个宏任务执行完
promise1
promise2     //执行全部的微任务
setTimeout   //执行下一个宏任务
```

Promise里面有了一个新的概念：**microtask**。JS中分为两种任务类型：**macrotask**和**microtask**,在ECMAScript中，microtack可称为**jobs**,macrotask可称为**task**。

- macrotask（又称之为宏任务），可以理解是每次执行栈执行的代码就是一个宏任务（包括每次从事件队列中获取一个事件回调并放到执行栈中执行）
- 每一个task会从头到尾将这个任务执行完毕，不会执行其它。
- 浏览器为了能够使得JS内部task与DOM任务能够有序的执行，会在一个task执行结束后，在下一个 task 执行开始前，对页面进行重新渲染。

`task->渲染->task->...`

- microtask（又称为微任务），可以理解是在当前 task 执行结束后立即执行的任务。
- 也就是说，在当前task任务后，下一个task之前，在渲染之前。
- 所以它的响应速度相比setTimeout（setTimeout是task）会更快，因为无需等渲染。
- 也就是说，在某一个macrotask执行完后，就会将在它执行期间产生的所有microtask都执行完毕（在渲染前）。

`task->jobs->渲染->task->jobs...`

什么样的场景会形成macrotask和microtask呢？

- macrotask：主代码块，setTimeout，setInterval等（可以看到，事件队列中的每一个事件都是一个macrotask）
- microtask：Promise，process.nextTick、MutationObserver等。
>在node环境下，process.nextTick的优先级高于Promise__，也就是可以简单理解为：在宏任务结束后会先执行微任务队列中的nextTickQueue部分，然后才会执行微任务中的Promise部分。

根据线程理解：

- macrotask中的事件都是放在一个事件队列中的，而这个队列由`事件触发线程`维护。
- microtask中的所有微任务都是添加到微任务队列（Job Queues）中，等待当前macrotask执行完毕后执行，而这个队列由`JS引擎线程维护`。

总结运行机制：
>执行一个宏任务（栈中没有就从事件队列中获取）
执行过程中如果遇到微任务，就将它添加到微任务的任务队列中
宏任务执行完毕后，立即执行当前微任务队列中的所有微任务（依次执行）
当前宏任务执行完毕，开始检查渲染，然后GUI线程接管渲染
渲染完毕后，JS线程继续接管，开始下一个宏任务（从事件队列中获取）

![run](../assets/run.jpg)

#### Vue.nextTick实现原理

- vue用异步队列的方式来控制DOM更新和nextTick回调先后执行。
- microtask因为其高优先级特性，能确保队列中的微任务在一次事件循环前被执行完毕。
- 因为兼容性问题，vue不得不做了microtask向macrotask的降级方案。

>那么最优的microtask策略就是Promise了，而令人尴尬的是，Promise是ES6新增的东西，也存在兼容问题。所以vue就面临一个降级策略。
macrotask降级的方案依次是：setImmediate、MessageChannel、setTimeout.