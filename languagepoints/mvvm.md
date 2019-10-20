1.什么是mvvm
=
mvvm是model-view-viewmodel的缩写，model代表数据模型，定义数据修改和操作的业务逻辑
view代表ui组件，将数据模型转换成ui展现出来。viewmodel是同步view和model的对象

viewmodel通过双向数据绑定把view和model连接起来。且view和model之间的同步工作都是自动的，
开发者只需关注业务逻辑不需要手动操作dom。

2.mvvm和mvc的区别
=
mvc中controler需要手动编写大量dom操作，mvvm解决了mvc大量dom操作使页面加载慢
和model发生变化开发者需要主动更新到view

3.vue双向绑定的原理
=

Vue内部通过Object.defineProperty方法，
把data对象里每个数据的读写转化成getter/setter，
当数据变化时通知视图更新。

4.数据双向绑定
=

```js
<input type="text" id="userInput">
<script>
    // 数据双向绑定
    const obj = {};
    Object.defineProperty(obj, "a", {
        value: 12,
        writable: true,
        enumerable: true,
        configurable: true
    });
    let bValue = 88;
    Object.defineProperty(obj, "b", {
        enumrable: true,
        configurable: true,
        get: function () {
            return bValue;
        },
        set: function (value) {
            bValue = value;
            // 绑定输入框
            const userInput = document.getElementById("userInput");
            userInput.value = value;
        }
    });
</script>
```
