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
