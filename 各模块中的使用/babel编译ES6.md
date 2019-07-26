# 一、初始化一个webpack项目
```
npm init -y                     // 初始化项目，会添加一个package.json
npm install --save-dev webpack  // 下载webpack，并记录到package.json的devDependencies中
```
# 二、新建webpack.config.js文件
指定入口和出口
```
// webpack.config.js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  mode:'development'
};
```
# 三、集成babel
- babel-loader：在import或加载模块时，对es6代码进行预处理，es6语法转化为es5语法。
- babel.core：允许我们去调用babel的api，可以将js代码分析成ast（抽象语法树），方便各个插件分析语法进行相应的处理.
- babel-preset-env：指定规范，比如es2015，es2016，es2017，latest，env（包含前面全部）
- babel-polyfill：它效仿一个完整的ES2015+环境，使得我们能够使用新的内置对象比如 Promise，静态方法比如Array.from 或者 Object.assign, 实例方法比如 Array.prototype.includes 和生成器函数（提供给你使用 regenerator 插件）。为了达到这一点， polyfill 添加到了全局范围，就像原生类型比如 String 一样。
- babel-runtime和babel-plugin-transfoem-runtime：与babel-polyfill作用一样，使用场景不一样。
## 1.安装babel-loader和babel-core
```
npm install --save-dev babel-loader babel-core
```
在webpack.config.js中添加
```
module: {
    rules: [
      {
        test: /\.js$/, exclude: /node_modules/, loader: "babel-loader"
        // test 符合此正则规则的文件，运用 loader 去进行处理，除了exclude 中指定的内容
      }
    ]
  }
```
## 2.安装babel-preset-env
```
npm install babel-preset-env --save-dev
```
新建.babelrc文件
```
//babelrc
{
  "presets": ["env"]
}

```
## 3.安装babel-polyfill
此依赖用于开发应用，会在全局添加新的方法，会污染全变量。
```
npm install --save babel-polyfill
```
在入口文件本文为index.js的顶部添加
```
import "babel-polyfill"
```
在webpcak.config.js中将babel-polyfill添加到entry数组中
```
module.exports = {
  entry: ['babel-polyfill', './src/index.js'],
  ...
```
最终webpack.config.js为
```
const path = require('path');

module.exports = {
  entry: ['babel-polyfill', './src/index.js'],
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  mode:'development',
  module: {
    rules: [
      {
        test: /\.js$/, exclude: /node_modules/, loader: "babel-loader"
        // test 符合此正则规则的文件，运用 loader 去进行处理，除了exclude 中指定的内容
      }
    ]
  }
};
```
到目前位置，用于开发应用的babel环境已经配置好了。
执行
```
npx webpack --config webpack.config.js
```
便可在dist中看到打包出来的bundle.js文件
## 4.babel-runtime 和 babel-plugin-transform-runtime
如果你不是开发应用，而是开发提供给第三方利用的框架的话，将第3步的polyfill改为这两个依赖。它们在局部添加新方法，不污染全局变量
```
npm install --save-dev babel-runtime babel-plugin-transform-runtime
```
.babelrc文件
```
{
  "presets": ["env"],
  "plugins": ["transform-runtime"]
}
```