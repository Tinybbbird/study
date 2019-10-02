1.浅克隆
=
```javascript
var lilei={
  sname:"lilei",
  sage:11
}
function clone(obj){
  var newObj={};
  for(var key in obj){
    newObj[key]=obj[key];
  }
  return newObj;
}
var lilei2=clone(lilei);
console.log(lilei2==lilei);//false 2个对象比较的是地址
```
浅克隆：如果包含内嵌的对象或数组则不再复制副本 
```javascript
var lilei={
  sname:"lilei",
  sage:11,
  address:{
    prov:"浙江",
    city:"杭州"
  }
}
function clone(obj){
  var newObj={};
  for(var key in obj){
    newObj[key]=obj[key];
  }
  return newObj;
}
var lilei2=clone(lilei);
lilei2.address.city="温州";
console.log(lilei.address.city)//温州
```
给lilei添加address对象，修改lilei2的city，lilei跟着修改，说明lilei和lilei2的address对象地址相同

2.深克隆
=
深克隆：如果包含内嵌的对象或数组也复制副本
```javascript
var lilei={
  sname:"lilei",
  score:null,
  friends:["jack","rose"],
  sage:11,
  address:{
    prov:"浙江",
    city:"杭州"
  }
}
function clone(obj){
  if(obj===null){
    return null;
  }
  if({}.toString.call(obj)==="[object Array]"){
    var newArr=[];
    newArr=obj.slice();
    return newArr;
  }
  var newObj={};
  for(var key in obj){
    
    if(typeof obj[key]!=="object")
    newObj[key]=obj[key];
    else{
      newObj[key]=clone(obj[key]);
    }
  }
  return newObj;
}
var lilei2=clone(lilei);
lilei2.address.city="温州";
console.log(lilei.address.city)//杭州
```
当一个对象的属性是内嵌对象时，再次给内嵌对象调用clone函数，使lilei2和lilei的address指向不同地址，形成深克隆。但null,数组等特殊情况需单独判断
