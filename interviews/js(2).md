javascript笔试题整理（二）
=

1.介绍 JavaScript 的原型，原型链？有什么特点？
 
    原型：
    每一个构造函数都拥有一个prototype属性，这个属性指向一个对象，也就是原型对象
    原型对象默认拥有一个constructor属性，指向指向它的那个构造函数
    每个对象都拥有一个隐藏的属性__proto__，指向它的原型对象
    原型链：
    JavaScript中所有的对象都是由它的原型对象继承而来。而原型对象自身也是一个对象，它也有自己的原型对象，这样层层上溯，就形成了一个类似链表的结构，这就是原型链
    所有原型链的终点都是Object函数的prototype属性。Objec.prototype指向的原型对象同样拥有原型，不过它的原型是null，而null则没有原型
    
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

