# webpack-dev-server
webpack-dev-server可以看作一个服务者，它的主要工作就是接收浏览器的请求，然后将资源返回。   
主要功能：  
- live-reloading(自动刷新)
- hot-module-replacement(模块热替换)  

两大职能：
- 令Webpack进行模块打包，并处理打包结果的资源请求。
- 作为普通的Web Server，处理静态资源文件请求。  
  
webpack-dev-server只是将打包结果放在内存中，不会写入实际的bundle.js，在每次webpack-dev-server接收到请求时都只是将内存中的打包结果返回给浏览器。
## 安装使用
1. 安装
```
npm install webpack-dev-server --save-dev
```
2. 为了便捷启动webpack-dev-server，在package.json中添加一个dev指令：
```
......
  "scripts":{
      "build":"webpack",
      "dev":"webpack-dev-server"
  },
......
```
3. 在webpack.config.js中配置  
添加一个devServer对象
```
module.exports={
    entry:'./src/index.js',
    output:{
        filename:'./bundle.js',
    },
    mode:'develpoment',
    devServer:{
        publicPath:'/dist',
    },
};
```