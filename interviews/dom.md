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


    
    
