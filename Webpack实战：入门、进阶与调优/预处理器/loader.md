# loader概述
loader的本质为以下公式表示：  
output=loader(input)
# loader的配置
## 引入
1. 安装
```
npm install css-loader
```
2. 引入工程中
   ```
   module.exports={
       //...
       module:{
           rules:[{
               test:/\.css$/,
               use:['css-loader'],
           }],
       },
   };
   ```
   **module.rules代表了模块的处理规则**  
   - test可接收一个正则表达式或者一个元素为正则表达式的数组，只有正则匹配上的模块才会使用这条规则。在本例中以/\.css$/来匹配所有以.css结尾的文件。
   - use可接收一个数组，数组包含该规则所使用的loader。在本例中只配置了一个css-loader，在只有一个loader时也可以简化为字符串“css-loader”。
## loader options
```
rules:[
    {
        test:/\.css$/,
        use:[
            'style-loader',
            {
                loader:'css-loader',
                options:{
                    //css-loader配置项
                }
            }
        ]
    }
]
```
## 更多配置
### 1. exclude(排除)与include(包含)
exclude和include同时存在时，exclude的优先级更高
```
//对include中的子目录进行排除
rules:[
    {
        test:/\.css$/,
        use:['style-loader','css-loader'],
        exclude:/src\/lib/,
        include:/src/,
    }
],
```
### 2. resource(被加载模块)与issuer(加载者)
用于更加精确地确定模块规则的作用范围
```
//index.js
import './style.css';
```
resource是/path/of/app/style.css  
issuer是/path/of/app/index.js  
  
  例子
  ```
  rules:[
      {
          test:/\.css$/,
          use:['style-loader','css-loader'],
          exclude:/node_modules/,
          issuer:{
              test:/\.js$/,
              include:/src/pages/,
          },
      }
  ],
  ```
  只有在/src/pages/目录下面的JS文件引用CSS文件，规则才会生效