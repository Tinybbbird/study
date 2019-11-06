webpack
=

1.入口(entry)

入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。

进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

```js
module.exports = {
  entry: './path/to/my/entry/file.js'
};
```

2.输出(output)

配置 output 选项可以控制 webpack 如何向硬盘写入编译文件。注意，即使可以存在多个入口起点，但只指定一个输出配置。

```js
const config = {
  output: {
    filename: 'bundle.js',//filename 用于输出文件的文件名。
    path: __dirname + '/dist'//目标输出目录 path 的绝对路径。
  }
};

module.exports = config;
```

3.loader

loader 用于对模块的源代码进行转换。loader 可以使你在 import 或"加载"模块时预处理文件

你可以使用 loader 告诉 webpack 加载 CSS 文件，或者将 TypeScript 转为 JavaScript。

为此，首先安装相对应的 loader：

```js
npm install --save-dev css-loader
npm install --save-dev ts-loader
```

```js
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.ts$/, use: 'ts-loader' }
    ]
  }
};
```

4.plugins

插件目的在于解决 loader 无法实现的其他事。

```js
const HtmlWebpackPlugin = require('html-webpack-plugin'); //通过 npm 安装
 plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
```

webpack-dev-server为你提供了一个简单的 web 服务器，并且能够实时重新加载(live reloading)。让我们设置以下：

```
npm install --save-dev webpack-dev-server
```

package.json:

```js
"scripts": {
      "start": "webpack-dev-server --open",
      "build": "webpack"
    },
```
