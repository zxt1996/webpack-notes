## 1.rem与px
### 1.1 rem
rem是相对于根元素<html>，这样就意味着，我们只需要在根元素确定一个参考值，这个参考值设置为多少，完全可以根据您自己的需求来定。  
1rem等于html根元素设定的font-size的px值，假如我们在css里面设定下面的css。
```
html{font-size:14px}
```
## 2.移动端CSS中px自动转化成rem
### 2.1安装
```
npm i px2rem-loader
```
### 2.2 在style-loader、css-loader后使用px2rem-loader。
```
{
    loader: 'px2rem-loader',
    options: {
        remUnit: 40,      // rem转换比例
        remPrecision: 8  // rem保留几位数 
    }
}
```