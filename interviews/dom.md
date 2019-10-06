dom面试题整理
=
1.getElementsByXXX与querySelectorAll()的效率

    getElementsByxxx返回一种动态集合，不实际存储数据，每次访问集合时，都重新查找dom树。
    querySelectorAll()返回非动态集合，即使反复访问集合，也不重新查找dom树。dom树发生变化，也无法获知。
    getElementsByxxx效率高，如果只需一个条件查找到就使用getElementsByxxx
2.onready和onload的区别
    
    onready先比onload执行
    onready是页面解析（html和js）完成后执行，onload是在页面所有元素加载完成后执行
    onready可以执行多个
    $(document).ready(function(){})可简写为$(function(){ ... })
3.dom元素e的e.getAttribute(propName)和e.propName的区别

    e.getAttribute(propName)能够获取标准属性和自定义扩展属性，不能获得状态属性，如checked,disabled
    e.propName能够访问标准属性和状态属性，不能获得自定义属性
4.描述浏览器的渲染过程？

    解析 HTML 构建 DOM(DOM 树)，并行请求 css/image/js
    CSS 文件下载完成，开始构建 CSSOM(CSS 树)
    CSSOM 构建结束后，和 DOM 一起生成 Render Tree(渲染树)
    布局(Layout)：计算出每个节点在屏幕中的位置
    显示(Painting)：通过显卡把页面画到屏幕上   
5.重绘和回流（重排）的区别和关系？

    重绘：当渲染树中的元素外观（如：颜色）发生改变，不影响布局时，产生重绘
    重排：当渲染树中的元素的布局（如：尺寸、位置、隐藏/状态状态）发生改变时，产生重绘回流
    注意：JS 获取 Layout 属性值（如：offsetLeft、scrollTop、getComputedStyle 等）也会引起回流。因为浏览器需要通过回流计算最新值
    重排必将引起重绘，而重绘不一定会引起重排
6.script 的位置是否会影响首屏显示时间？

    在解析 HTML 生成 DOM 过程中，js 文件的下载是并行的，不需要 DOM 处理到 script 节点。因此，script 的位置不影响首屏显示的开始时间。
    浏览器解析 HTML 是自上而下的线性过程，script 作为 HTML 的一部分同样遵循这个原则
    因此，script 会延迟 DomContentLoad，只显示其上部分首屏内容，从而影响首屏显示的完成时间
7.DOM的浏览器兼容性问题
    
（1）事件模型：

  DOM：1.捕获 2.目标触发 3.由内向外：冒泡

  IE8：1.目标触发 2.冒泡

（2）事件绑定：

  DOM：```elem.addEventListener("click",function(){},false);```

  第三个参数capture:是否在捕获阶段提前触发 
  
  IE8:```elem.attachEvent("onclick",function(){})```
  
（3）移除事件：

  DOM:```elem.removeEventListener('click', handler, false);```
  
  IE8：```elem.detachEvent(event, handler);```
  
（4）获取事件对象：
  
  DOM:```function(e){}```
  
  IE8:不会自动传入事件对象e，```function(){var e=window.event}```
  
（5）阻止冒泡：

  DOM：```e.stopPropagation()```
  IE8:```e.cancelBubble=true```
  
  (6)阻止默认行为：
  
  DOM：```e.preventDefault()```
  IE8:事件处理函数中：```return false;``` ;```e.returnValue = false'```
  
  (7)获取目标元素：
  
  DOM：```e.target```
  IE8:```e.srcElement```
  
  定义一个函数可以支持所有浏览器和处理函数的绑定：
  ```js
  function bindEvent(elem,eventName,handler){
      if(typeof elem.attachEvent!=="function"){
        elem.addEventListener(eventName,handler)
      }else{
        elem.attachEvent("on"+eventName,function(){
          var e=window.event;
          e.target=e.srcElement;
          handler.call(this,e);
        })
      }
    }
    bindEvent(btn,"click",function(e){
      var target=e.target;
    })
  ```
8.事件委托

事件委托是指将事件绑定目标元素的到父元素上，利用冒泡机制触发该事件

优点：

-   事件监听对象个数少，节省大量内存占用
-   可以将事件应用于动态添加的子元素上

缺点： 使用不当会造成事件在不应该触发时触发

示例：

```js
ulEl.addEventListener('click', function(e){
    var target = event.target || event.srcElement;
    if(!!target && target.nodeName.toUpperCase() === "LI"){
        console.log(target.innerHTML);
    }
}, false);
```
9.如何获得一个 DOM 元素的绝对位置？
    
    elem.offsetLeft：返回元素相对于其定位父级左侧的距离
    elem.offsetTop：返回元素相对于其定位父级顶部的距离
    
10.判断是什么浏览器？
```js
var ua=navigator.userAgent;
    document.write(`<h1>${ua}</h1>`);
    var browser, version;
    //如果ua中包含MSIE，说明一定是IE浏览器
    if(ua.indexOf("MSIE")!=-1){
      browser="IE"
    }else if(ua.indexOf("Trident")!=-1){
      browser="IE",version=11;
    }
    //如果ua中包含Firefox，说明一定是Firefox浏览器
    else if(ua.indexOf("Firefox")!=-1){
      browser="Firefox"
    }
    //如果ua中包含OPR，说明一定是OPR浏览器
    else if(ua.indexOf("OPR")!=-1){
      browser="OPR"
    }
    //如果ua中包含Edge，说明一定是Edge浏览器
    else if(ua.indexOf("Edge")!=-1){
      browser="Edge"
    }
    //如果ua中包含Chrome，说明一定是Chrome浏览器
    else if(ua.indexOf("Chrome")!=-1){
      browser="Chrome"
    }
    //如果ua中包含Safari，说明一定是Safari浏览器
    else if(ua.indexOf("Safari")!=-1){
      browser="Safari"
    }
    document.write(`<h1>浏览器:${browser}</h1>`);

    //如果还不知道浏览器的版本号时: 
    if(version===undefined){
      //获得浏览器名称在ua中的位置
      var i=ua.indexOf(browser);
      //i跳过浏览器名称的长度，再+1
      i+=browser.length+1;
      //从新i位置向后截3位，再parseFloat转为浮点数
      version=parseFloat(ua.slice(i,i+3));
    }
    document.write(`<h1>版本号:${version}</h1>`)
```

11.定义一个函数，遍历一个指定父元素下所有后代元素
```js
function getChildren(parent){
	//arguments.callee->getChildren
	//1. 先获得当前父元素下的所有直接子元素
	var children=parent.children;
	//遍历所有直接子元素
	for(var child of children){
		//输出当前子元素的标签名
		console.log( child.nodeName )
		        //元素的标签名
		//2. 如果child有下级直接子元素
		if(child.children.length>0){
			//就对child继续调用getChildren
			arguments.callee(child)
		}
	}
}
//查找body下所有后代元素
getChildren(document.body);
```
