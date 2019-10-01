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
