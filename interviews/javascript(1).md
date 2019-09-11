javascript笔试题整理（一）
=
1.
```
var a={n:1};
var b=a;//b指向{n:1}
a={n:2};
a.x=a;
console.log(a.x);//{n:2,x:{n:2,...}}
console.log(b.x);//undefined
```
    a.x意为a添加属性x,x指向a;b中没有属性x

2.
```
var a={n:1};
var b=a;//b指向{n:1}
a.x=a={n:2};
console.log(a.x);//undefined
console.log(b.x);//{n:2}
```
    a.x=a={n:2};  
    程序从左向右执行,先给{n:1}添加属性x,再让a指向新的对象{n:2},返回新的地址给x。所以b仍为{n:1,x},x指向{n:2}
    =赋值操作不但会将右边的值保存到左边的变量中,而且会返回等号右边的值

3.定义一个新类型People,继承Animal类型。(ES5)
```
function Animal(name,age){
  this.name=name;
  this.age=age;
}
Animal.prototype.intr=function(){
  console.log(`i'm ${this.name},i'm ${this.age}`)
}
```
    答案： 
    function People(name,age,className){
      Animal.call(this,name,age);//将animal的this替换为people中正确的this
      this.className=className;
    }
    People.prototype.run=function(){
      console.log(`${this.name} run`);
    }
    var lilei=new People("Lilei",11,"初一2班");
    Object.setPrototypeOf(People.prototype,Animal.prototype);//让people的原型对象继承Animal的原型,lilei能够调用intr方法
    lilei.run()//输出lilei run
    lilei.intr() //输出i'm lilei,i'm 11.
