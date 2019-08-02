# optimization.SplitChunks
```
//webpack.config.js
module.exports={
    entry:{
        filename:'foo.js',
        publicPath:'/dist/',
    },
    mode:'development',
    optimization:{
        splitChunks:{
            chunk:'all',
        },
    },
};

//foo.js
import React from 'react';
import ('./bar.js');
document.write('foo.js',React.version);

//bar.js
import React from 'react';
console.log('bar.js',React.version);
```
原本我们打包的结果应该是foo.js及0.foo.js（异步加载bar.js的结果，后面会介绍），但是由于SplitChunks的存在，又生成了一个vendors~main.foo.js，并且把react提取到了里面。  
- chunks:'all'含义：  
  SplitChunks将会对所有的chunks生效(默认情况下，SplitChunks只对异步chunks生效，并且不需要配置)
# 默认情况下的提取条件
- 提取后的chunk可被共享或者来自node_modules目录
- 提取后的javascript chunk体积大于30kB,CSS chunk体积大于50kB
- 在按需加载过程中，并行请求的资源最大值小于等于5.
- 在首次加载时，并行请求的资源数最大值小于等于3.
  
# 配置
SplitChunks的默认配置
```
splitChunks:{
    chunks:"async",
    minSize:{
        javascript:30000,
        style:50000,
    },
    maxSize:0,
    minChunks:1,
    maxAsyncRequests:5,
    maxInitialRequests:3,
    automaticNameDelimiter:'~',
    name:true,
    cacheGroups:{
        vendors:{
            test:/[\\/]node_modules[//\]/,
            priority:-10,
            },
        default:{
            minChunks:2,
            priority:-20,
            reuseExistingChunk:true,
        },
    },
},
```
## 1. 匹配模式--chunks
- async(默认)，只提取异步chunk
- initial,只对入口chunk生效
  ```
  splitChunks: {
    cacheGroups: {
        commons: {
            name: "commons",
            chunks: "initial",
            minChunks: 2
        }
    }
  }
  ```
  这会创建一个commons代码块，这个代码块包含所有被其他入口(entrypoints)共享的代码。

- all，两种模式同时开启
```
  splitChunks: {
      cacheGroups: {
        commons: {
            test: /[\\\\/]node_modules[\\\\/]/,
            name: "vendors",
            chunks: "all"
        }
    }
}
```
这会创建一个名为vendors的代码块，它会包含整个应用所有来自node_modules的代码。  

## 2.匹配条件
- minSize  
  表示抽取出来的文件在压缩前的最小大小，默认为 30000；
- maxSize  
  表示抽取出来的文件在压缩前的最大大小，默认为 0，表示不限制最大大小；
- minChunks  
  可以接受一个数字，当设置minChunks为n时，只有该模块被n个入口同时引用时才会进行提取。
- maxAsyncRequests  
  最大的按需(异步)加载次数，默认为 5；
- maxInitialRequests  
  最大的初始化加载次数，默认为 3；
- automaticNameDelimiter  
  抽取出来的文件的自动生成名字的分割符，默认为 ~；
## 3.命名
配置项name默认为true，它意味着SplitChunks可以根据cacheGroups和作用范围自动为新生成的chunk命名，并以automaticNameDelimiter分隔。如vendors~a~b~c.js意思是cacheGroups为vendors，并且该chunk是由a、b、c三个入口chunk所产生的。
## 4.cacheGroups
分离chunks的规则
- vendors  
  用于提取所有的node_modules中符合条件的模块
- default  
  作用于被多次引用的模块
- test  
  表示要过滤 modules，默认为所有的 modules，可匹配模块路径或 chunk 名字，当匹配的是 chunk 名字的时候，其里面的所有 modules 都会选中；
- priority  
  表示抽取权重，数字越大表示优先级越高。因为一个 module 可能会满足多个 cacheGroups 的条件，那么抽取到哪个就由权重最高的说了算；
- reuseExistingChunk  
  表示是否使用已有的 chunk，如果为 true 则表示如果当前的 chunk 包含的模块已经被抽取出去了，那么将不会重新生成新的。