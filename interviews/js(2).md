javascript笔试题整理（二）
=

1.介绍 JavaScript 的原型，原型链？有什么特点？
 
    原型：
    每一个构造函数都拥有一个prototype属性，这个属性指向一个对象，也就是原型对象
    原型对象默认拥有一个constructor属性，指向指向它的那个构造函数
    每个对象都拥有一个隐藏的属性__proto__，指向它的原型对象
    原型链：
    JavaScript中所有的对象都是由它的原型对象继承而来。而原型对象自身也是一个对象，它也有自己的原型对象，这样层层上溯，
    就形成了一个类似链表的结构，这就是原型链
    所有原型链的终点都是Object函数的prototype属性。
    Objec.prototype指向的原型对象同样拥有原型，不过它的原型是null，而null则没有原型
    
2.JavaScript 如何实现一个类，怎么实例化这个类？

（1）. 构造函数法（this + prototype） -- 用 new 关键字 生成实例对象
     - 缺点：用到了 this 和 prototype，编写复杂，可读性差

```js
  function Mobile(name, price){
     this.name = name;
     this.price = price;
   }
   Mobile.prototype.sell = function(){
      alert(this.name + "，售价 $" + this.price);
   }
   var iPhone7 = new Mobile("iPhone7", 1000);
   iPhone7.sell();
```
（2）. ES6 语法糖 class -- 用 new 关键字 生成实例对象

```js
     class Point {
       constructor(x, y) {
         this.x = x;
         this.y = y;
       }
       toString() {
         return '(' + this.x + ', ' + this.y + ')';
       }
     }

  var point = new Point(2, 3);
```
3. Javascript 如何实现继承？

（1）构造函数绑定：使用 call 或 apply 方法，将父对象的构造函数绑定在子对象上

```js
function Cat(name,color){
 　Animal.apply(this, arguments);
 　this.name = name;
 　this.color = color;
}
```

（2）实例继承：将子对象的 prototype 指向父对象的一个实例

```js
Cat.prototype = new Animal();//此时cat.prototype.constructor指向了Animal
Cat.prototype.constructor = Cat;//重新指回原来的构造函数Cat
```

（3）拷贝继承：如果把父对象的所有属性和方法，拷贝进子对象

```js
function extend(Child, Parent) {
　　　var p = Parent.prototype;
　　　var c = Child.prototype;
　　　for (var i in p) {
　　　   c[i] = p[i];
　　　}
　　　c.uber = p;
}
```

（4）原型继承：将子对象的 prototype 指向父对象的 prototype

```js
function extend(Child, Parent) {
    var F = function(){};
    　F.prototype = Parent.prototype;
    　Child.prototype = new F();
    　Child.prototype.constructor = Child;
    　Child.uber = Parent.prototype;
}
```

（5）ES6 语法糖 extends：class ColorPoint extends Point {}

```js
class ColorPoint extends Point {
    constructor(x, y, color) {
        super(x, y); // 调用父类的constructor(x, y)
        this.color = color;
    }
    toString() {
        return this.color + ' ' + super.toString(); // 调用父类的toString()
    }
}
```
4. javascript 创建对象的几种方式？

(1)对象字面量的方式

```js
person={firstname:"Mark",lastname:"Yun",age:25,eyecolor:"black"};
```
(2)用new创建

```js
var person=new Object();
person.name="Mark";
person.age="25";
```
(3)构造函数创建
```js
function Pet(name,age,hobby){
    this.name=name;//this作用域：当前对象
    this.age=age;
    this.hobby=hobby;
    this.eat=function(){
        alert("我叫"+this.name+",我喜欢"+this.hobby+",是个程序员");
    }
}
var maidou =new Pet("麦兜",25,"coding");//实例化、创建对象
maidou.eat();//调用eat方法
```
5.防抖和节流的实现

防抖：只要不是最后一次触发就不执行异步操作
```js
var timer;
window.onscroll=function(){
  if(timer!==undefined){
    clearTimeout(timer);
  }
  //当200ms内未发生滚动时发送请求
  timer=setTimeout(() => {
    console.log("发送请求")
  }, 200);
}
```
节流：第一次发送请求后，只要响应没回来就不发送第二个
```js
var canClick=true;//定义开关变量
btn.onclick=function(){
  if(canClick){//如果当前开关开着，说明可以单击
    canClick=false;
    console.log("发送请求")
    setTimeout(() => {
      console.log("加载完成")
      canClick=true;
    }, 3000);
  }
}
```
6.new操作符干了什么

（1） 新建一个空对象

（2）让子对象的_ _proto_ _属性指向妈妈的原型对象

（3）用new调用构造函数。将构造函数中的this，都吸引到new上！
    然后通过强行赋值的方式！给新对象添加构造函数的属性和方法！
    
（4）返回新对象的地址给变量保存起来。

7.js数组的原生方法

    arr.concat(arr1, arr2, arrn);//返回一个由当前数组和其它若干个数组或者若干个非数组值组合而成的新数组
    arr.join(",");//连接所有数组元素组成一个字符串
    arr.sort(func);//对数组元素进行排序，并返回当前数组
    arr.pop();//删除数组的最后一个元素，并返回这个元素
    arr.push(e1, e2, en);//在数组的末尾增加一个或多个元素，并返回数组的新长度
    arr.shift();//删除数组的第一个元素，并返回这个元素
    unshift(e1, e2, en);//在数组的开头增加一个或多个元素，并返回数组的新长度
    arr.reverse();//颠倒数组中元素的排列顺序
    arr.slice(start, end);//抽取当前数组中的一段元素组合成一个新数组
    arr.splice(index, count, e1, e2, en);//在任意的位置给数组添加或删除任意个元素
    arr.indexOf(el);//返回数组中第一个与指定值相等的元素的索引，如果找不到这样的元素，则返回 -1   
    forEach(): 为数组中的每个元素执行一次回调函数,最终返回 undefined
    every(): 如果数组中的每个元素都满足测试函数，则返回 true，否则返回 false
    some(): 如果数组中至少有一个元素满足测试函数，则返回 true，否则返回 false
    filter(): 将所有在过滤函数中返回 true 的数组元素放进一个新数组中并返回
    map(): 返回一个由回调函数的返回值组成的新数组

        
8.为什么 JS 是单线程,而不是多线程
 
    单线程是指 JavaScript 在执行的时候，有且只有一个主线程来处理所有的任务。
    目的是为了实现与浏览器交互。
    我们设想一下，如果 JavaScript 是多线程的，现在我们在浏览器中同时操作一个 DOM，
    一个线程要求浏览器在这个 DOM 中添加节点，
    而另一个线程却要求浏览器删掉这个 DOM 节点，那这个时候浏览器就会很郁闷，
    他不知道应该以哪个线程为准。所以为了避免此类现象的发生，降低复杂度，
    JavaScript 选择只用一个主线程来执行代码，以此来保证程序执行的一致性。

9.js多线程worker的实现

    ```js
    //worker.js
    onmessage=function(e){
      for(var i=0;i<1000000000;i++){
        this.postMessage(i);
      }
    }
    //worker.html
    var w=new Worker("work.js");
    w.onmessage=function(e){
      console.log(e.data);
    }
    w.postMessage("");
    ```
    
10.什么是闭包？

  简单来说，闭包就是能够读取其他函数内部变量的函数

```js
function Person() {
  var name = 'hello'
  function say () {
      console.log(name)
  }
  return say()
}
Person() // hello
```

  用途：

    最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中

  注意点：

    由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露

11.js 异步加载的方式有哪些？

  defer 和 async
  
     defer 并行加载 js 文件，会按照页面上 script 标签的顺序执行
     async 并行加载 js 文件，下载完成立即执行，不会按照页面上 script 标签的顺序执行
  
  动态创建 DOM 方式（用得最多）：document.createElement('script');
  
12.跨域问题的产生，怎么解决它

    由于浏览器的 同源策略，在出现 域名、端口、协议有一种不一致时，就会出现跨域，属于浏览器的一种安全限制。

  解决跨域问题有很多种方式，常用的就是以下几种：

    jsonp 跨域：动态创建script，再请求一个带参网址实现跨域通信.缺点就是只能实现 get 一种请求
    跨域资源共享（CORS）：只服务端设置Access-Control-Allow-Origin即可，前端无须设置，若要带cookie请求：前后端都需要设置

13.apply()、call()和 bind() 是做什么的，它们有什么区别

  相同点：三者都可以改变 this 的指向
  
  不同点：call ：在这一次调用函数时，临时替换一次this为任意指定的对象！
                要调用的函数.call(替换this的对象, ...)
         
  apply: apply和call用法几乎完全一样。只不过，要求所有实参值都要放在一个数组中整体传入。
         
  bind: 不调用函数，而是基于原函数，创建一个新函数副本。并永久替换新函数中的this为指定的对象。
  
14. 什么是内存泄漏，哪些操作会造成内存泄漏

  内存泄漏：是指一块被分配的内存既不能使用，又不能回收，直到浏览器进程结束

  可能造成内存泄漏的操作：

    意外的全局变量
    闭包
    循环引用
    被遗忘的定时器或者回调函数
    
15.什么是事件代理，它的原理是什么

    事件代理：通俗来说就是将元素的事件委托给它的父级或者更外级元素处理

    原理：利用事件冒泡机制实现的

    优点：只需要将同类元素的事件委托给父级或者更外级的元素，不需要给所有元素都绑定事件，减少内存空间占用，提升性能; 动态新增的元素无需重新绑定事件

16.promise的了解

    Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大.所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果 

    Promise 对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和 rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态
```js
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
})
```
    Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数
```js
promise.then(function(value) {
  // success
}, function(error) {
  // failure
})
```
    then 方法返回的是一个新的Promise实例

    Promise.prototype.catch 用于指定发生错误时的回调函数,具有“冒泡”性质，会一直向后传递，直到被捕获为止。
    也就是说，错误总是会被下一个catch语句捕获
```js
getJSON('/post/1.json').then(function(post) {
  return getJSON(post.commentURL);
}).then(function(comments) {
  // some code
}).catch(function(error) {
  // 处理前面三个Promise产生的错误
});
```
    catch 方法返回的还是一个 Promise 对象，因此后面还可以接着调用 then 方法

17.async
    
    async   await  可按照传统的同步指定的代码一样，编写异步代码顺序执行
    
    async 函数返回一个 Promise 对象，可以使用 then 方法添加回调函数。当函数执行的时候，一旦遇到 await 就会先返回，等到异步操作完成，再接着执行函数体内后面的语句

    async 函数内部 return 语句返回的值，会成为 then 方法回调函数的参数

    async 函数返回的 Promise 对象，必须等到内部所有 await 命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到 return 语句或者抛出错误

    async 函数内部抛出错误，会导致返回的 Promise 对象变为 reject 状态。抛出的错误对象会被 catch 方法回调函数接收到
```js
function timeout(ms) {
  return new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}

async function asyncPrint(value, ms) {
  await timeout(ms);
  console.log(value);
}

asyncPrint('hello world', 50);
```
    await 命令: await 命令后面是一个 Promise 对象，返回该对象的结果。如果不是 Promise 对象，就直接返回对应的值
```js
async function f() {
  // 等同于
  // return 123;
  return await 123;
}

f().then(v => console.log(v))
// 123
```
    await 命令后面是一个thenable对象（即定义then方法的对象），那么await会将其等同于 Promise 对象.也就是说就算一个对象不是Promise对象，但是只要它有then这个方法， await 也会将它等同于Promise对象
    
18.export 与 export default有什么区别

    export 与 export default 均可用于导出常量、函数、文件、模块等

    在一个文件或模块中，export、import 可以有多个，export default 仅有一个

    通过 export 方式导出，在导入时要加 { }，export default 则不需要

    使用 export default命令，为模块指定默认输出，这样就不需要知道所要加载模块的变量名; export 加载的时候需要知道加载模块的变量名

    export default 命令的本质是将后面的值，赋给 default 变量，所以可以直接将一个值写在 export default 之后

