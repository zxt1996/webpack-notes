# mini-css-extract-plugin
- 按需加载CSS  
  将CSS提取到单独的文件中，为每个包含CSS的JS文件创建一个CSS文件。
  ```
  //app.js
  import './style.css';
  import ('./next-page');
  document.write('app.js<br/>');

  //next-page.js
  import './next-page.css'
  document.write('Next page.<br/>');

  /*style.css*/
  body{background-color:#eee;}

  /*next-page.css*/
  body{background-color:#999;}

  //webpack.config.js
  const MiniCssExtractPlugin=require('mini-css-extract-plugin');
  module.exports={
      entry:'./app.js';
      output:{
          filename:'[name].js';
      },
      mode:'development',
      module:{
          rules:[{
              test:/\.css$/,
              use:[
                  {
                      loader:MiniCssExtractPlugin.loader,
                      options:{
                          publicPath:'../',
                      },
                  },
                  'css-loader'
              ],
          }],
      },
      plugins:[
          new MiniCssExtractPlugin({
              filename:'[name].css',
              chunkFilename:'[id].css',
          })
      ]
  };
  ```
  - 配置publicPath来指定异步CSS的加载路径
  - 在plugin设置中，指定同步加载的CSS资源名（filename）,还要指定异步加载的CSS资源名(chunkFilename)。
