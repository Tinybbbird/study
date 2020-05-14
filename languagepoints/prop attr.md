jquery中

对于HTML元素本身就带有的可见的固有标准属性如id,src,type等，在处理时，可以使用attr和prop方法。

对于HTML元素我们自己自定义的DOM属性，在处理时，使用attr方法。

boolean类型的状态属性，如 checked, selected 或者 disabled 使用prop()

DOM元素中附加的属性selectedIndex, tagName, nodeName, nodeType, ownerDocument, defaultChecked, defaultSelected等也用prop()


```js
<input type="text" name="text" disabled>
```

```js
$('input').prop('disabled')  //true
$('input').attr('disabled')  //disabled
$('input').attr('name')  //text
$('input').prop('name')  //text
$('input').prop('nodeName')  //INPUT
$('input').prop('nodeName')  //undefined
```

