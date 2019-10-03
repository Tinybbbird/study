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
