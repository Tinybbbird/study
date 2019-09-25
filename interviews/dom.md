dom面试题整理
=
1.getElementsByXXX与querySelectorAll()的效率
```
getElementsByxxx返回一种动态集合，不实际存储数据，每次访问集合时，都重新查找dom树。
querySelectorAll()返回非动态集合，即使反复访问集合，也不重新查找dom树。dom树发生变化，也无法获知。
getElementsByxxx效率高，如果只需一个条件查找到就使用getElementsByxxx
