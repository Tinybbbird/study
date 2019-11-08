# this指向总结

### 1.普通函数调用

普通函数this指向全局变量

```js
let a=1;
function fn(){
    console.log(this.a);//undefined
}
fn();
```
    使用let定义的全局变量不会变成window对象的属性
    
```js
var a=1;
function fn(){
    console.log(this.a);//1
}
fn();
```

### 2.对象中调用

this指向调用的那个对象

```js
var b=2222;
var obj={
    a:111,
    fn:function(){
        alert(this.a);//111
        alert(this.b);//undefined
    }
}
obj.fn();
```

### 3.构造函数调用

this指向创建的新对象

```js
var Student = function(){
  this.age=18
}
var lilei=new Student();
lilei.age=20;
console.log(lilei.age)//20
var han=new Student();
console.log(han.age)//18
```

### 4.箭头函数调用

箭头函数里面的 this 指向外面的环境。

```js
var obj={
    a:222,
    fn:function(){    
        setTimeout(function(){console.log(this.a)})
    }
};
obj.fn();//undefined
```

    所有回调函数，真正被调用时，前边是没有任何"对象."前缀，所以this指向window
    
```js
var obj={
    a:222,
    fn:function(){    
        setTimeout(()=>{console.log(this.a)})
    }
};
obj.fn();//222
```

    箭头函数里this转向外部，所以要向上层作用域查找.
    setTimeout的上层作用域是fn。而fn里面的this指向obj ，所以setTimeout里面的箭头函数的this指向obj.所以输出 222 
    
### 5.事件处理函数中

    btn.onclick=function(){... }  中this指向当前btn

### 6.apply和call中

apply和this都能强行改变this的指向

```js
    let obj1={
        a:222
    };
    let obj2={
        a:111,
        fn:function(){
            alert(this.a);
        }
    }
    obj2.fn.call(obj1);//222
```
    
    obj1使用obj2的方法并将this指向obj1
