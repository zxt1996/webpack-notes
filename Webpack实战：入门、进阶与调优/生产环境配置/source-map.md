# source map
source map指的是将编译、打包、压缩后的代码映射回源代码的过程。
## 配置
- javascript  
  在webpack.config.js中添加devtool即可
  ```
  module.exports={
      //...
      devtool:'source-map',
  };
  ```
- CSS、SCSS、Less等  
  需要添加额外的source map配置项
  ```
  const path=require('path');
  module.exports={
      //...
      devtool:'source-map',
      module:{
          rules:[
              {
                  test:/\.scss$/,
                  use:[
                      'style-loader',
                      {
                          loader:'css-loader',
                          options:{
                              sourceMap:true,
                          },
                      },{
                          loader:'sass-loader',
                          options:{
                              sourceMap:true,
                          },
                      }
                  ],
              }
          ],
      },
  };
  ```