html
=

1.Doctype作用？标准和兼容模式各有什么区别？

    告知浏览器的解析器用什么文档标准解析这个文档
    标准模式的排版和JS运作模式都是以浏览器支持的最高标准运行，兼容模式以向后兼容的方式显示

2.页面导入样式时，使用link和@import的区别
    
    @import只能引入css文件，link既能引入css又能引入其他文件比如vue脚手架index.html的.ico图标
    加载页面时，link引入的css同时加载；@import引入的css在页面加载完成后加载
    @import的优先级低于link的css

3.浏览器内核的理解
  
    分为渲染和JS引擎。
    渲染引擎负责读取网页内容（html,xml,图像,css）并输出
    JS引擎解析和执行js实现网页的动态效果
  
4.常见的浏览器内核有哪些？

    Trident：IE,
    Gecko:firefox
    Presto
    Webkit:safari,chrome,opera
    
5.H5新特性
  
    图像渲染canvas，svg
    影音vidio,audio
    数据存储sessionStorage,localStorage
    语义化标签article,footer,header,nav,section
    表单控件date,time,email,url,search
    webworker专用线程,websocket通信,Geolocation地理定位
    
6.HTML语义化的理解？
  
    用正确的标签做正确的事
    让页面内容结构化，便于浏览器解析
    利于SEO
    便于阅读维护
    
7.cookie,sessionStorage,localStorage的区别
 
    相同点：都存储在客户端
    不同点：存储大小。cookie大小不超过4k.sessionStorage,localStorage可以达到5M
           有效时间。localStorage永久存储。浏览器关闭后数据也不丢失除非主动删除
                    sessionStorage在浏览器窗口关闭后自动删除
                    cookie在设置的过期时间之前一直有效
           数据与服务器的交互方式。cookie会自动传递到服务器。sessionStorage,localStorage不会自动传递。仅在本地保存
           
8.iframe有哪些缺点？
    
    iframe会阻塞主页面的Onload事件
    不利于SEO
    影响页面的并行加载

9.如何实现浏览器内多个标签页的通信？

  （1）cookie+setInterval方法
    
```javascript
//send.html
send.onclick=function(){
    document.cookie=`msg=${msg.value.trim()}`
}
//rec.html
function getkey(key){ //将cookie里的字符串转化为对象
    var cookies=JSON.parse(`{"${document.cookie.replace(/=/g,'":"')
    .replace(/;\s+/g,'", "')}"}`)
    return cookies[key];
}
setInterval(function(){
    recMsg.innerHTML=getkey(msg);}
,500)
```

  (2)localStorage
  
```javascript
//send.html
send.onclick=function(){
    localStorage.setItem("msg",msg.value.trim());
}
//rec.html
window.addEventListener("storage",function(){
  recMsg.innerHTML=localStorage.getItem("msg");
})//只要修改localstorage的值会自动触发其他页面中的storage事件
```

  (3)websocket

```javascript
//server.js
var webSocketServer = require("ws").Server;
//创建webSocketServer对象实例
var wss=new webSocketServer({port:8080});
//创建保存所有连接到服务器的客户端对象数组
var clients=[];
//为服务器添加connection事件监听
wss.on('connection',function (client){
  if(clients.indexOf(client)===-1){
    clients.push(client);
    //为每个client对象绑定message事件
    client.on('message',function(msg){
      console.log(msg);      
      for(var c of clients){
        if(c!=client){
          c.send(msg);
        }
      }
    })
  }
})
//send.html
<body>
  <input type="text" id="msg">
  <button id="send">发送</button>
  <script>
    var ws=new WebSocket("ws://localhost:8080");
    send.onclick=function(){
      if(msg.value.trim()!=="")
      ws.send(msg.value.trim());
    }
  </script>
</body>
//rec.html
<body>
  <h1>收到消息：<span id="recMsg"></span></h1>
    <script>
        //建立到服务器的连接
        var ws=new WebSocket("ws://localhost:8080");
        //当连接被打开，注册接收消息的处理函数
        ws.onopen=function(event){
        //当有消息发过来时显示消息
          ws.onmessage=function(event){
            recMsg.innerHTML=event.data;
          }
        }
    </script>
</body>
```

  （4）SharedWorker
  
```javascript
//worker.js
//存储多个worker共享的数据
let data="";
//为新创建的worker绑定onconnect事件处理函数
onconnect=function(e){
  //获得当前连接上的客户端对象
  var client=e.ports[0];
  client.onmessage=function(e){
    if(e.data===""){//消息内容为空，客户端获取data数据
      client.postMessage(data);
    }else{//消息不为空，将消息存在data中
      data=e.data;
    }
  }
}
//send.html
var worker=new SharedWorker("worker.js");
worker.port.start();
send.onclick=function () {
  if(msg.value.trim()!==""){
    worker.port.postMessage(msg.value.trim())
  }
}
//rec.html
var worker=new SharedWorker("worker.js");
//2.当返回data触发message事件. data的值保存入事件对象e的data属性中
worker.port.addEventListener("message",function(e){
  recMsg.innerHTML=e.data;
})
worker.port.start();
//接收端反复向work.js发送空消息获取data的值
setInterval(() => {
  worker.port.postMessage("");
},500);
```

CSS
=

1.display:none 和 visibility:hidden区别

    display:none:会让元素完全从渲染树中消失，不占任何空间；visibility:hidden只是内容不可见
    display:none非继承属性 visibility:hidden继承属性
    修改display会造成文档重排，修改visibility只会造成元素重绘
    读屏幕不会读取display:none的内容，会读取visibility:hidden的内容
    
2.响应式的实现
```
@media screen and (min-width: 1200px){
}
@media screen and (min-width: 992px) and (max-width: 1199px){
      .container{width:970px}
      .left,.right{width:20%}
      .middle{width:60%}
      .my-img{width:25%}
    }
    /*针对pad 750px; 1-3-*屏幕的媒体查询*/
    @media screen and (min-width: 768px) and (max-width: 991px){
      .container{width:750px;color:green}
      .left{width:25%}
      .middle{width: 75%}
      .right{display: none;}
      .my-img{width: 25%}
    }
    /*针对phone 100% 100%-100%-0屏幕的媒体查询*/
    @media screen and (max-width: 767px){
      .container{width: 100%;color:blue}
      .left,.middle{width: 100%}
      .right{display: none}
      .my-img{width:50%}
    }
```
