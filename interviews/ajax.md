Ajax
=

1.请写出至少5种常见的状态码及代表的意义

    200:请求成功
    303：重定向，告知客户端应该用另一个URL来获取资源
    400：请求格式错误
    404：请求失败，服务器无法找到所请求的URL
    500：服务器出错

2.ajax的理解

    ajax是异步的js和xml,是一种创建交互式网页的网络开发技术，可以实现页面的异步请求和局部刷新。
    
3.用post方式异步向服务器端提交数据时，需要在发送请求前设置什么？提交的数据放在什么位置？

    xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
    数据放在send（）括号中
    
4.异步请求数据的步骤分为哪几步？
```js    
var xhr = new XMLHttpRequest();
xhr.onreadystatechange=function(){
if(xhr.readyState==4&&xhr.status==200){
var resText = xhr.responseText;
}
}
xhr.open(method,url,true);
xhr.send(body);
```

5.如何理解json

    JSON是JS对象的一种表现方式，即以js对象的数据格式表现出来的字符串，JSON中的两个api如下：
    将JSON字符串转换成JSON对象 JSON.parse()
    将JSON对象转换成JSON字符串 JSON.stringify()
    
6.http和https的区别？

    http传输的数据都是未加密的，也就是明文的
    https协议是由http和ssl协议构建的可进行加密传输和身份认证的网络协议，比http协议的安全性更高。
    两者使用不同的链接方式，端口也不同，一般而言，http协议的端口为80，https的端口为443
    
7.ajax的优缺点

    优点：
    1.局部刷新页面,减少用户心理和实际的等待时间,带来更好的用户体验。
    2.减轻服务器的压力,按需取数据,最大程度的减少冗余数据请求。
    3.基于xml标准化,并被广泛支持,不需安装插件。
    4.促进页面和数据的分离。
    缺点：
    1.AJAX破坏了浏览器的Back和History功能
    2.AJAX安全问题
    3.对搜索引擎支持较弱
    4.破坏程序的异常处理机制
    5.AJAX不是很好支持移动设备
    
