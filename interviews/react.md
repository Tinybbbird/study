1.什么是jsx？

是javascript xml的简写，使html文件容易理解，并且能够提高性能。

```js
render(){
  return (
    <div>
      <h1>hello</h1>
    </div>
  )
}
```

2.与ES5相比，react的ES6语法有何不同？

（1）require与import

```js
//ES5
var react = require('react');

//ES6
import react from 'react';
```
  
  (2)export与exports
  
```js
//ES5
module.exports = component;

//ES6
export default component;
```

  (3)component和function

```js
//ES5
var mycomponent = React.createClass({
  render: function() {
    return 
      <h3>hello</h3>
  }
})

//ES6
class mycomponent extends React.Component {
  render() {
    return <h3>hello</h3>
  }
}
```

  (4)state
  
```js
//ES5
var mycomponent = React.createClass({
  getInitialState: function() {
    return {name: 'world'}
  }
  render: function() {
    return 
      <h3>hello,{this.state.name}</h3>
  }
})

//ES6
class mycomponent extends React.Component {
  constructor() {
    super();
    this.state = {name: 'world'}
  }
  render() {
    return <h3>hello,{this.state.name}</h3>
  }
}
```

3.什么是props

react属性的简称，是只读组件。他们总是在整个应用中从父组件传递到子组件

4.react中的状态

状态是数据的来源，是可变的，可以通过this.state（）访问

5.如何更新状态

使用this.setState()

6.无状态组件和有状态组件

无状态的函数创建的组件是无状态组件，不需管理状态state,数据通过props传入

有状态组件包含state且随着事件而发生变化。使用state来存储数据，通常带有生命周期。

7.react的生命周期
```
 1)首次渲染相关函数
	contstructor()
	getDerivedStateFromProps()   用于将this.props转为this.state	
	render()
	componentDidMount()	用于初始化组件中的数据，如异步获取服务器端数据
 2)二次渲染相关函数(props属性更改、setState状态修改等)
	getDerivedStateFromProps()   需要返回转换得到state对象或null
	shouldComponentUpdate()   需要返回true或false
	render()
	componentDidUpdate()
 3)组件卸载相关函数
	componentWillUnmount()  用于销毁组件创建的长期存在数据，如定时器
```

8.react中keys的作用

追踪哪些列表中的元素被修改的辅助标识，用于识别唯一的虚拟dom元素，优化渲染。

9.受控组件和非受控组件

受控组件没有维持自己的状态，数据由父组件控制，通过props获取当前值（表单元素）

非受控组件保持自己的状态，数据由dom控制，refs用于获取当前值

10.纯组件

编写的最简单的组件，通过控制shouldComponentUpdate函数减少render调用次数减少性能损耗





