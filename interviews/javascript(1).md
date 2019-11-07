javascript笔试题整理（一）
=
1.
```javascript
var a={n:1};
var b=a;//b指向{n:1}
a={n:2};
a.x=a;
console.log(a.x);//{n:2,x:{n:2,...}}
console.log(b.x);//undefined
```
    a.x意为a添加属性x,x指向a;b中没有属性x

2.
```javascript
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
```javascript
function Animal(name,age){
  this.name=name;
  this.age=age;
}
Animal.prototype.intr=function(){
  console.log(`i'm ${this.name},i'm ${this.age}`)
}
```
``` javascript
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
```
4.
```javascript
function change(o){
  o.name="xixi";
  o={};//创建一个对象，将地址给局部变量o
  o.name="xiaoohong";//局部变量o增加name属性
}
var o={name:"xiaolv",age:11};
change(o);
console.log(o);//{name:"xixi",age:11}
```
    全局变量o存放地址，调用change后，局部变量o和全局变量o都指向同一对象。
5.数组去重
```javascript
var arr=[1,2,3,2,1,4];
var hash={};
for(var i=0;i<arr.length;i++){
  hash[arr[i]]=1;
}
var res=[];
var i=0;
for(res[i++] in hash);
console.log(res);//[1,2,3,4]
```
6.去掉数组中非数字字符，并给每个数字+1
```javascript
var arr=[1,2,3,'a',4,'b'];
```
```
for(var i=arr.length-1;i>=0;i--){{
  if(typeof arr[i]==="number"){
    arr[i]++;
  }else{
    arr.splice(i,1);//删除非数字
  }
}
console.log(arr);//[2,3,4,5]
```
7.在2个`排好序`的数组中，高效率地找出相同元素，放入新数组：
```javascript
var arr1=[1,3,7,9,12,37,45];
var arr2=[2,4,9,13,45,88,92];
```
```
for(var i=0,j=0,result=[];i<arr1.length&&arr2.length;){
  if(arr1[i]<arr2[j]){
    i++;
  }else if(arr1[i]>arr2[j]){
    j++;
  }else{
    result.push(arr1[i]);
    i++;j++;
  }
}
```
8.找出一个`排好序`的数组中，两个元素相加为19的元素组合
```
var arr=[1,2,4,6,7,11,12,15,17];
```
```
for(var i=0,j=arr.length-1;i<j;){
  if(arr[i]+arr[j]>19){
    j--;
  }else if(arr[i]+arr[j]<19){
    i++;
  }else{
    console.log(arr[i],arr[j]);
    i++;j--;
  }
}
```
9.
```javascript
var length=10;
function fn(){
  console.log(this.length);//this指向全局变量，所以为window
}
var obj={
  length:5,
  method(fn){
    fn();
    arguments[0]{};
  }
}
obj.method(fn,1);//10 2
```
    arguments = {
      0:fn,    //function fn(){console.log(this.length);}
      1:第二个参数 1，
      length:2
    }
    arguments的length=2,arguments[0]()可以理解为arguments.0()，所以this指向arguments
10.
```javascript
function fn(a){
  console.log(a);
  var a=2;
  function a(){}
  console.log(a);
}
fn(1);
```
    变量提升，最后会变成
    var a;
    function a;
    console.log(a);//function a(){}
    a=2;console.log(a);//2
11.
```javascript
var f=true;
if(f===true){//js中没有块级作用域
  var a=10;
}
function fn(){
  var b=20;//b为局部变量
  c=30;//强行给未声明的变量赋值，c为全局变量
}
fn();
console.log(a);//10
console.log(c);//30
console.log(b);//报错
```
12.
```
if('a' in window){ //window对象中是否含有a属性
  var a=10;//变量提升，a为全局变量
}
alert(a);//10
```
13.
```javascript
var a=10;
a.pro=10;//new Number(10).pro=10；自动释放。pro属性随着释放
console.log(a.pro+a);//a.pro为undefined，所以结果为NaN
var s="hello";
s.pro='world';
console.log(s.pro+s);//undefinedworld
```
    每当对原始类型的值调用方法或属性时，都会自动创建包装类型对象，且该对象在调用后自动释放
14.实现点击对应链接alert相应编号
```
<body>
    <a href="#">1</a>
    <a href="#">2</a>
    <a href="#">3</a>
    <a href="#">4</a>
</body>
```
```javascript
var as=document.getElementsByTagName("a");
var i=0;
for(var a of as){
    i++;
    a.onclick=(function(i){
        return function(){
        alert(i);
        }
    })(i);
}
```
15.将url参数解析为一个对象
```javascript
var search="?key0=0&key1=1";//location.search
search=serch.slice(1);
var strs=search.split("&");
var params={};
for(var str of strs){
    var arr=str.split("=");
    params[arr[0]]=arr[1];
}
console.log(params);
```
16.统计一个字符串中字符出现的个数，最多为几次
```javascript
var str="helloworld";
var dict={};
for(var i=0;i<str.length;i++){
    if(dict[str[i]]===undefined){
        dict[str[i]]=1;
    }else{
        dict[str[i]]++;
    }
}
console.log(dict);
var max,count=0;
for(var key in dict){
    if(dict[key]>count){
        max=key;
        count=dict[key];
    }
}
console,log(max,count);
```
17.
```javascript
var a=10;
var obj={
    a:20,
    intr:function(){
        var a=30;
        console.log(this.a);
    }
}
obj.intr();//20
var intr=obj.intr;
intr();//10
```
18.[1,2,3].map(parseInt)

    parseInt(string,radix),radix表示进制，省略 radix 或 radix = 0，则数字将以十进制解析
    map 每次为 parseInt 传 3 个参数(elem, index, array)，
    因此，map 遍历 ["1", "2", "3"]，相应 parseInt 接收参数如下
    parseInt('1', 0);  // 1
    parseInt('2', 1);  // NaN
    parseInt('3', 2);  // NaN

19.
```js
function Foo(){
  Foo.a=function(){
    console.log(1)
  }
  this.a=function(){
    console.log(2)
  }
}
Foo.prototype.a=function(){
  console.log(3)
}
Foo.a=function(){//函数也是一个对象，给函数添加方法a
  console.log(4)
}
Foo.a(); //4
let obj=new Foo();//调用Foo,创建obj使Foo中的this指向obj
obj.a();//2   
Foo.a();//1
```

20.
```js
var x=0;
var foo={
  x:1,
  bar(){
    console.log(this.x)
    var that=this;
    return function(){
      console.log(this.x)
      console.log(that.x)//that为外部函数局部变量指向foo
    }
  }
}
foo.bar();//1 this指向foo
foo.bar()();//1 0 1
```
    foo.bar()()相当于function(){ console.log(this.x);console.log(that.x)}(),即匿名函数自调用
    
21.使用一次性定时器模拟周期性定时器
```js
<h1 id="h1">10s后自动关闭窗口</h1>
  <a id="a1" href="javascript:;">启动</a>
  <script>
    (function(){
      var s = 10;
      var timer;
      function sub() {
        timer = setTimeout(() => {
          s--;
          h1.innerHTML = `${s}s后自动关闭`;
          if(s>0) {
            sub()
          }else {
            timer = null;
          }
        }, 1000);
      }
      
      a1.onclick = function(){
        if(timer == null) {
        sub();
        this.innerHTML = "停止"
        }else{
          clearTimeout(timer);
          timer = null;
          this.innerHTML = "启动"
        }
      }
    })()
  </script>
```

22.
```js
var a=0;
function (){
  a=b=3
}
setTimeout(function(){
  console.log(a,b)//3,3
},0)
fun()
//定时器的回调函数只能在回调队列等待，等待主程序执行完
```

23.
```js
for(var i=0;i<5;i++){
    setTimeout(()=>{
        console.log(i) //5,5,5,5,5
    },50)
}
```
    只要函数不调用，就不去执行函数体内容。for循环生成5个定时器，主程序执行完后，此时i=5，所以输出5个5

```js
for(let i=0;i<5;i++){
    setTimeout(()=>{
        console.log(i) //0，1，2，3，4
    },50)
}
```
    let相当于匿名函数自调用
