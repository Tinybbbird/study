cors
=
（1）跨域资源共享（CORS）

CORS（Cross-Origin Resource Sharing）跨域资源共享，定义了必须在访问跨域资源时，浏览器与服务器应该如何沟通。CORS背后的基本思想就是使用自定义的HTTP头部让浏览器与服务器进行沟通，从而决定请求或响应是应该成功还是失败。

服务器端对于CORS的支持，主要就是通过设置Access-Control-Allow-Origin来进行的。如果浏览器检测到相应的设置，就可以允许Ajax进行跨域的访问。

    只需要在后台中加上响应头来允许域请求！在被请求的Response header中加入以下设置，就可以实现跨域访问了！

    //指定允许其他域名访问
    'Access-Control-Allow-Origin:*'//或指定域
    //响应类型
    'Access-Control-Allow-Methods:GET,POST'
    //响应头设置
    'Access-Control-Allow-Headers:x-requested-with,content-type'

jsop
=
JSONP由两部分组成：回调函数和数据。回调函数是当响应到来时应该在页面中调用的函数，而数据就是传入回调函数中的JSON数据。

JSONP的原理：通过script标签引入一个js文件，这个js文件载入成功后会执行我们在url参数中指定的函数，并且会把我们需要的json数据作为参数传入。所以jsonp是需要服务器端的页面进行相应的配合的。（即用javascript动态加载一个script文件，同时定义一个callback函数给script执行而已。）

在js中，我们直接用XMLHttpRequest请求不同域上的数据时，是不可以的。但是，在页面上引入不同域上的js脚本文件却是可以的，jsonp正是利用这个特性来实现的。 例如：
```
<script type="text/javascript">
    function dosomething(jsondata){
    //处理获得的json数据
    }
</script>
<script src="http://example.com/data.php?callback=dosomething"></script>
```

js文件载入成功后会执行我们在url参数中指定的函数，并且会把我们需要的json数据作为参数传入。所以jsonp是需要服务器端的页面进行相应的配合的。

JSONP的优缺点

    JSONP的优点是：它不像XMLHttpRequest对象实现的Ajax请求那样受到同源策略的限制；
    它的兼容性更好，在更加古老的浏览器中都可以运行，不需要XMLHttpRequest或ActiveX的支持；
    并且在请求完毕后可以通过调用callback的方式回传结果。

    JSONP的缺点则是：它只支持GET请求而不支持POST等其它类型的HTTP请求；
    它只支持跨域HTTP请求这种情况，不能解决不同域的两个页面之间如何进行JavaScript调用的问题。
