1.typeof
```javascript
console.log(
  typeof 1,//number
  typeof "hello", //string
  typeof true, //boolean
  typeof null, //object
  typeof undefined,  //undefined
  typeof function(){},  //function
  typeof {sname:"abc"}, //object
  typeof [1,2,3],  //object
  typeof NaN  //number
)
```

判断是否为NULL(===)
```javascript
var obj=undefined;
console.log(obj==null);//ture,因为undefined会被转换成null
console.log(obj===null);//flase, ===屏蔽了自动类型转换
```
    结论：只要判断null,undefined,0,"",false都要用===和!==
如何判断是否为NaN(isNaN)
```javascript
var a=10,b=NaN;
console.log(isNaN(a),isNaN(b));//false,true
```
判断对象和数组方法（Array.isArray(),Object.prototype.toString.call()）
```
var a={},b=[];
console.log(Array.isArray(a),Array.isArray(b));//false,true
console.log(Object.prototype.toString.call(a)==="[object Array]", 
Object.prototype.toString.call(a)==="[object Array]");//false,true
```
//Object类的toString能够输出一个对象的类型名,Array类的toString方法能够输出内容以逗号分隔，所以需要call去调用Object.prototype.toString(),可简写为{}.toString.call()
